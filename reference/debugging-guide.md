---
description: >-
  How to find out why your ServiceNow script isn't working — reading logs,
  session debugging, the browser console, inspecting objects, and a
  step-by-step "my script didn't run" checklist.
---

# Debugging Guide

Every developer spends more time figuring out *why code doesn't work* than writing it. This page teaches the debugging skills the rest of the course assumes — so when something breaks (and it will), you have a method instead of a guess.

{% hint style="success" %}
This is also an **AI-readiness skill.** When an AI generates a script that misbehaves, debugging is how you find the flaw and correct it. You cannot trust AI output you cannot diagnose.
{% endhint %}

## The debugging mindset

Before touching code, answer three questions:

1. **Where does this code run?** Client (browser) or server (instance)? That decides which tools you use.
2. **Did it run at all?** Or did it run and produce the wrong result? These are completely different problems.
3. **What did it actually see?** Print the real values — do not assume. Most bugs are a value that is not what you thought.

## Server-side debugging

### 1. Read the system log

Server-side `gs.info()`, `gs.warn()`, and `gs.error()` all write to the system log.

- Go to **System Logs → System Log → All** (or filter to **Script Log Statements**).
- Filter by **Source**, **Level**, and **Created** to find your entry fast.
- In **Scripts - Background**, output prints directly on the results screen.

```javascript
gs.info('Reached point A. priority=' + current.getValue('priority'));
gs.info('Records found: {0}', gr.getRowCount());   // {0} placeholder form
```

### 2. Inspect objects with JSON.stringify

Logging an object directly prints `[object Object]`. Stringify it to see inside:

```javascript
var data = { isHigh: true, count: 3 };
gs.info('data = ' + JSON.stringify(data));   // data = {"isHigh":true,"count":3}
```

For a GlideRecord, log the specific fields you care about (`gr.getValue('field')`) — not the whole object.

### 3. Session debugging (the debug toolbar)

Turn on debugging for **your session only** so you see execution as it happens:

- **System Diagnostics → Session Debug → Debug Business Rule** — shows every Business Rule that fires on a transaction, in order, with timing.
- **Debug Business Rule (Details)** — adds the SQL and field values.
- **Debug Log to Screen** — streams `gs.log`/`gs.info` output to the bottom of the page as you click around.
- Turn it **off** when done (or use **Disable All Debugging**) — it is noisy and slows the instance.

### 4. The JavaScript debugger

**System Diagnostics → Script Debugger** lets you set breakpoints in server-side scripts (Business Rules, Script Includes) and step through line by line, inspecting variables. Use it when logging is not enough and you need to watch state change.

## Client-side debugging

### 1. The browser console

Client Scripts run in the browser, so use the browser DevTools (**F12** → **Console**).

```javascript
console.log('onChange fired. newValue=', newValue);
console.log('g_form priority =', g_form.getValue('priority'));
```

- **Console** tab — your `console.log` output and any red JavaScript errors.
- **Network** tab — watch the request a **GlideAjax** call makes; inspect the response payload the server sent back.
- Remember: `g_form.getValue()` returns a **string**, so `newValue === '1'`, not `=== 1`.

### 2. Field messages instead of alerts

`g_form.showFieldMsg('priority', 'debug: ' + value, 'info')` surfaces a value on the form itself — handy when the console is cluttered.

## "My script didn't run" — the checklist

When code seems to do nothing at all, walk this list in order. Nine times out of ten the answer is here:

- [ ] **Is it active?** Check the **Active** checkbox on the Business Rule / Client Script / UI Policy record.
- [ ] **Right table and scope?** Confirm the script is on the table you are testing, in the scope you are working in.
- [ ] **Does the condition pass?** A **When to run** condition or `condition` field that evaluates false means the script silently skips. Temporarily clear it to test.
- [ ] **Right trigger?** Business Rule: is it **before/after/async/display** as intended, and on **insert/update** as needed? Client Script: **onLoad/onChange/onSubmit** — and for onChange, the correct **field**?
- [ ] **Order matters.** A lower **Order** number runs earlier. Another rule may be overriding your change, or running first.
- [ ] **Client vs server confusion.** Are you calling a server API (`GlideRecord`, `gs`) from a Client Script? That will not work — use **GlideAjax**.
- [ ] **GlideAjax wiring.** Did you set `sysparm_name` to the method, and does the Script Include extend **AbstractAjaxProcessor** with **no custom `initialize()`**? (The most common GlideAjax failure.)
- [ ] **Caching / hard refresh.** After changing a Client Script or UI Script, do a hard refresh (or clear the browser cache) so the old version is not still loaded.
- [ ] **Errors swallowed?** Check the system log (server) and the browser console (client) for an exception that stopped execution before your line.

## Common errors and what they really mean

| Symptom | Likely cause |
| --- | --- |
| GlideAjax callback gets `undefined` or the whole XML | Script Include declared its own `initialize()`, or you read `response` wrong — use `getXMLAnswer(fn)` |
| Comparison never true even though the value "looks" right | Comparing a **string** to a **number** (`getValue()` returns a string) |
| Client Script "does nothing" | Wrong event type, wrong field on onChange, or trying to use a server API client-side |
| `[object Object]` in the log | Logged an object without `JSON.stringify()` |
| REST call returns null body / throws | Did not check `getStatusCode()`, or `JSON.parse()` not wrapped in try/catch |
| Business Rule change is lost | Called `current.update()` inside a before/after rule, or another rule with lower Order overwrote it |
| Query returns records the user shouldn't see | Used `GlideRecord` where `GlideRecordSecure` was needed (ACLs bypassed) |

## A repeatable debugging loop

1. **Reproduce** it reliably (same steps, same record).
2. **Locate** the layer — client or server — using the checklist above.
3. **Print** the real values (`gs.info` + `JSON.stringify`, or `console.log`).
4. **Narrow** it down: comment out or add logging until you find the last line that behaves as expected.
5. **Fix**, then **re-test** — including the case that used to fail.

## Continue the course

* **Back to:** [Welcome](../welcome.md)
* **Related:** [Cheat Sheet](cheat-sheet.md) · [Glossary](glossary.md)
