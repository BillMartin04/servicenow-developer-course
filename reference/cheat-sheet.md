---
description: >-
  A one-page quick reference for the ServiceNow scripting you use every day —
  GlideRecord, GlideSystem, g_form, GlideAjax, Business Rule timing, and the
  rules that keep your code correct and AI-reviewable.
---

# Cheat Sheet & Quick Reference

Keep this page open while you build. It is a fast lookup for the APIs and rules covered across the course — not a tutorial. Every snippet here follows the same correctness rules the course enforces, so it is also a reference for **reviewing AI-generated code**: if an AI hands you something that breaks one of these rules, you now know it is wrong.

{% hint style="info" %}
Server-side scripts default to **ES5** (`var`, `function` — no arrow functions, no `let`/`const`) unless the script runs in an ES-latest context. When in doubt, write ES5-safe.
{% endhint %}

## GlideRecord (server-side querying)

```javascript
var gr = new GlideRecord('incident');
gr.addQuery('active', true);            // field = value
gr.addQuery('priority', '<=', 2);       // with operator
gr.addEncodedQuery('active=true^priority<=2'); // encoded query string
gr.orderByDesc('sys_created_on');
gr.setLimit(50);
gr.query();
while (gr.next()) {
  gs.info(gr.getValue('number'));       // getValue() ALWAYS returns a String
  gr.setValue('state', '2');
  gr.update();                          // or gr.setWorkflow(false); gr.update();
}
```

| Method | What it does |
| --- | --- |
| `new GlideRecord(table)` | Start a query/record on a table |
| `addQuery(field, [op,] value)` | Add a condition (AND) |
| `addOrCondition(...)` | OR against the previous query |
| `addEncodedQuery(str)` | Apply an encoded query string |
| `query()` / `next()` | Run the query / advance the cursor |
| `get(sys_id)` or `get(field, value)` | Fetch a single record; returns `true`/`false` |
| `getValue(field)` | **Returns a String** (or `null`) |
| `getDisplayValue(field)` | Human-readable value (e.g. reference display) |
| `setValue(field, value)` | Set a field in memory |
| `insert()` / `update()` / `deleteRecord()` | Persist changes; `insert()` returns the new sys_id |
| `setLimit(n)` / `getRowCount()` | Cap rows / count (avoid counting large tables) |

{% hint style="warning" %}
**Correctness rules.** `getValue()` returns a **string**, so compare with `'1'` not `1`. Use **`GlideRecordSecure`** instead of `GlideRecord` when a query must honour ACLs (e.g. anything driven by user input). Call `setWorkflow(false)` only when you deliberately want to skip Business Rules/notifications on an update.
{% endhint %}

## GlideSystem (`gs`) — server-side utilities

```javascript
gs.info('Message with {0} and {1}', a, b);   // logging with placeholders
gs.warn('...'); gs.error('...');
gs.getUserID();  gs.getUserName();            // current user
gs.hasRole('admin');
gs.nowDateTime();  new GlideDateTime();
gs.getProperty('my.property', 'default');
gs.eventQueue('my.event', current, param1, param2); // fire an event (async work)
gs.addInfoMessage('Saved');  gs.addErrorMessage('Failed'); // UI messages
```

## g_form (client-side form API)

```javascript
g_form.getValue('priority');              // returns a String
g_form.setValue('state', '2');
g_form.getReference('caller_id', callback); // async — pass a callback
g_form.setMandatory('short_description', true);
g_form.setReadOnly('category', true);
g_form.setVisible('subcategory', false);
g_form.showFieldMsg('email', 'Invalid', 'error');
g_form.addErrorMessage('...'); g_form.addInfoMessage('...');
g_form.isNewRecord();
```

{% hint style="warning" %}
Never query the database directly from a Client Script. For server data, call a **GlideAjax** Script Include (see below) or use `g_form.getReference()` with a callback. Synchronous server calls freeze the browser.
{% endhint %}

## GlideAjax (client → server bridge)

**Client Script:**

```javascript
var ga = new GlideAjax('AjaxValidator');        // name of the Script Include
ga.addParam('sysparm_name', 'isPriorityHigh');  // which method to run
ga.addParam('sysparm_priority', g_form.getValue('priority'));
ga.getXMLAnswer(function (response) {            // ASYNC — always prefer this
  var result = JSON.parse(response);             // response is a STRING
  if (result.isHigh) { g_form.addErrorMessage('High priority!'); }
});
```

**Script Include (client-callable):**

```javascript
var AjaxValidator = Class.create();
AjaxValidator.prototype = Object.extendsObject(global.AbstractAjaxProcessor, {
  // DO NOT define initialize() — the parent wires getParameter()
  isPriorityHigh: function () {
    var priority = this.getParameter('sysparm_priority');
    return JSON.stringify({ isHigh: priority === '1' });  // return a STRING
  },
  type: 'AjaxValidator'
});
```

{% hint style="danger" %}
**Three rules that break AI-generated GlideAjax most often:**
1. A client-callable Script Include **must not declare its own `initialize()`** — it overwrites the `AbstractAjaxProcessor` plumbing that makes `getParameter()` work.
2. GlideAjax **only ever returns a string** — `JSON.stringify()` server-side, `JSON.parse()` client-side.
3. Prefer async **`getXMLAnswer(callback)`** over the deprecated synchronous `getXMLWait()`.
{% endhint %}

## Business Rules — when they run

| Timing | Runs | Typical use |
| --- | --- | --- |
| **before** | Before the DB write, same transaction | Validate/transform `current` field values before save |
| **after** | After the DB write, same transaction | Act on the saved record (update related records) |
| **async** | Later, via the scheduler | Heavy or non-urgent work; keeps the save fast |
| **display** | When the form loads | Prepare `g_scratchpad` data for client scripts |

```javascript
(function executeRule(current, previous) {
  // 'current' = the record; 'previous' = pre-change values (not on inserts)
  if (current.priority == 1 && current.priority.changes()) {
    current.setValue('urgency', 1);   // before rule: change before it saves
  }
})(current, previous);
```

{% hint style="warning" %}
Do not call `current.update()` inside a **before/after** Business Rule on the same record — the platform is already saving it; just `setValue()` in a *before* rule. Use `.changes()` / `.changesTo()` to avoid running logic on every save.
{% endhint %}

## Script Includes — the reusable server layer

| Pattern | Base class / setup | Callable from |
| --- | --- | --- |
| Utility / on-demand | `Class.create()` + `initialize()` | Server script, other Script Includes |
| Client-callable (GlideAjax) | `Object.extendsObject(AbstractAjaxProcessor, {...})`, **no `initialize()`** | Client Script via GlideAjax |
| Extend another class | `Object.extendsObject(ParentClass, {...})` | Wherever the parent is used |

Keep **business logic in a service-layer Script Include**, not in the GlideAjax controller. The controller just reads params and delegates — that separation is what keeps code testable and safe for AI to extend.

## REST integration (scoped app)

```javascript
var r = new sn_ws.RESTMessageV2();          // scoped apps use sn_ws
r.setEndpoint('https://api.example.com/data');
r.setHttpMethod('GET');
r.setHttpTimeout(5000);                      // MILLISECONDS
var response = r.execute();
var code = response.getStatusCode();
if (code === 200) {
  try {
    var body = JSON.parse(response.getBody());
  } catch (e) {
    gs.error('Bad JSON from endpoint: ' + e.message);
  }
} else {
  gs.error('Endpoint returned ' + code);
}
```

{% hint style="danger" %}
Always check `getStatusCode()` before parsing, wrap `JSON.parse()` in try/catch, and **never hard-code credentials** — use a Connection & Credential alias. `setHttpTimeout()` is in milliseconds.
{% endhint %}

## The "is this code correct?" checklist

Use this to review your own — or an AI's — ServiceNow code:

- [ ] Comparisons treat `getValue()` / `g_form.getValue()` results as **strings**
- [ ] Client-callable Script Include has **no custom `initialize()`**
- [ ] GlideAjax uses `JSON.stringify`/`JSON.parse` and an **async** callback
- [ ] User-driven queries use **`GlideRecordSecure`** (ACL enforcement)
- [ ] Business Rule uses the right **timing** and `.changes()` guards
- [ ] REST checks status code, wraps parsing, uses a **credential alias**
- [ ] Server-side script is **ES5-safe** unless it runs in an ES-latest context
- [ ] Logic lives in a **service layer**, not copy-pasted into controllers

## Continue the course

* **Back to:** [Welcome](../welcome.md)
* **Related:** [Glossary](glossary.md) · [Debugging Guide](debugging-guide.md)
