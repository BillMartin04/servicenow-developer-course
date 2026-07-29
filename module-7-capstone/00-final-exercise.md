# Final Exercise: Build a Production-Ready App

_Part of Module 7 · Final Exercise & Assessment · Final Exercise & Assessment · [ServiceNow Developer Course](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)_

**Estimated time:** 4–6 hrs

## What you'll learn

By the end of this exercise you will have built one layered, secure, integrated ServiceNow application that demonstrates every skill in this course — and you will be able to defend its design.

{% hint style="warning" %}
**Read the [Scoring Rubric](01-scoring-rubric.md) before you start.** You are graded on architecture, not on whether the demo happens to work. Knowing the criteria up front changes how you build.
{% endhint %}

## The brief

> ### Weather-Aware Facilities Requests
>
> Staff raise facilities requests such as "office too cold". The business wants every request automatically enriched with the current outside temperature for that office location, wants the intake form to guide users intelligently, and wants high-impact requests prioritised automatically.

Your job is to deliver this as a professional would: layered, secure, testable, and observable.

## Target architecture

```
Catalog Item (UI)
      │  catalog client script + GlideAjax
      ▼
FacilitiesController      client-callable Script Include (AbstractAjaxProcessor)
      │  calls ↓
FacilitiesService         business logic: orchestration + priority rules
      │  injected ↓
      ├── WeatherGateway      RESTMessageV2 wrapper for the weather API
      └── FacilitiesRepository GlideRecordSecure read/write
                │
        GlobalErrorHandler    used by every layer
```

Each layer has exactly one job. The controller talks to the client, the service decides, the gateway talks outward, the repository talks to the database, and the error handler makes failures visible. This mirrors [Module 5 · Architecture](../module-5-architecture/README.md) and [Module 6 · Integrations](../module-6-integrations/README.md).

{% hint style="info" %}
**How to use this guide.** Every stage below has the same shape: **Goal → Do this → Code → Test it → Done when.** Build one stage, run its test, and only then move on. The code blocks are complete and copy-pasteable — but read them, don't just paste them. You will be asked to defend every line in the rubric.
{% endhint %}

---

## Stage 0 · Set up your scoped app and outbound access (~15 min)

Before any code, get the container and the network right. Skipping this is the number-one reason later stages "mysteriously" fail.

**Do this:**

1. In your [PDI](../module-0-getting-started/02-create-your-pdi.md), go to **System Applications → Studio** and create a new scoped application named **`Facilities Lab`** with scope **`x_course_lab`** (your actual prefix will look like `x_<instance>_facilities_lab` — that is fine, just be consistent).
2. Set the application's default access so Script Includes can be created inside it.
3. Allow the outbound weather call: go to **System Web Services → Outbound → REST Message** later in Stage 3, or simply confirm your instance can reach the public internet (PDIs can by default). No API key is needed for Open-Meteo.

{% hint style="warning" %}
**Scope prefixes are automatic.** When you create a table `facilities_request`, the platform stores it as `x_..._facilities_request`. Everywhere this guide writes `x_course_lab_facilities_request`, substitute your real prefix. Do not type the prefix yourself in the table-name field — the platform adds it.
{% endhint %}

**Done when:** Studio shows your scoped app and you are working *inside* that application scope (check the scope picker in the top nav).

---

## Stage 1 · Data model (~30 min)

**Goal:** a facilities request record that reuses the platform's Task fields.

**Do this:**

- Create table **`Facilities Request`** (`x_course_lab_facilities_request`), **extending Task** (`task`).
- Add three custom fields:

| Label | Column name | Type | Notes |
| --- | --- | --- | --- |
| Location | `location` | String (100) | e.g. "London HQ" |
| Request type | `request_type` | Choice | `heating`, `cooling`, `lighting`, `other` |
| Outside temp (°C) | `outside_temp_c` | Decimal | populated automatically |

- **Do not recreate** `priority`, `state`, `short_description`, or `assigned_to` — you inherit them from Task.

{% hint style="info" %}
**Why extend Task?** You get priority, state, assignment, work notes, and SLAs for free, and your records show up in standard task reporting. This is the single most common "senior vs junior" data-model decision on the platform.
{% endhint %}

**Test it:** open the table's list view, click **New**, fill `short_description` and `location`, and save.

**Done when:** the record saves and appears in the list with an auto-generated number.

---

## Stage 2 · Repository layer (~40 min)

**Goal:** the *only* place in the whole app that touches the database. No business logic here — just read and write, safely.

**Do this:** create a Script Include **`FacilitiesRepository`** (not client-callable) with `create`, `getById`, and `updateTemp`. Use **GlideRecordSecure** so the caller's ACLs are enforced, and use `getValue()` / `setValue()` rather than dot-walking.

```javascript
var FacilitiesRepository = Class.create();
FacilitiesRepository.prototype = {
    TABLE: 'x_course_lab_facilities_request',   // use YOUR scope prefix

    initialize: function () {},

    // Returns the sys_id of the new record, or null on failure.
    create: function (payload) {
        var gr = new GlideRecordSecure(this.TABLE);
        gr.initialize();
        gr.setValue('short_description', payload.short_description || 'Facilities request');
        gr.setValue('location', payload.location);
        gr.setValue('request_type', payload.request_type);
        var sysId = gr.insert();
        return sysId || null;
    },

    // Returns a plain object, or null if not found / not permitted.
    getById: function (sysId) {
        var gr = new GlideRecordSecure(this.TABLE);
        if (!gr.get(sysId)) return null;
        return {
            sys_id: gr.getUniqueValue(),
            location: gr.getValue('location'),
            request_type: gr.getValue('request_type'),
            outside_temp_c: gr.getValue('outside_temp_c')
        };
    },

    // Returns true on success.
    updateTemp: function (sysId, tempC) {
        var gr = new GlideRecordSecure(this.TABLE);
        if (!gr.get(sysId)) return false;
        gr.setValue('outside_temp_c', tempC);
        return !!gr.update();
    },

    type: 'FacilitiesRepository'
};
```

{% hint style="info" %}
**GlideRecordSecure vs GlideRecord.** `GlideRecordSecure` applies the running user's ACLs and field-level security. In a repository that will later be reached from a catalog form, that is exactly what you want — the database layer refuses to read or write anything the user themselves could not.
{% endhint %}

**Test it** in **All → Scripts - Background** (run in your app's scope):

```javascript
var repo = new x_course_lab.FacilitiesRepository();   // use YOUR scope
var id = repo.create({ location: 'London HQ', request_type: 'heating', short_description: 'Too cold' });
gs.info('Created: ' + id);
gs.info('Read back: ' + JSON.stringify(repo.getById(id)));
gs.info('Temp update ok? ' + repo.updateTemp(id, 4.2));
```

**Done when:** the log shows a created sys_id, the read-back object, and `Temp update ok? true`.

---

## Stage 3 · Gateway layer (~50 min)

**Goal:** the *only* place that talks to the outside world. It must never throw a raw error at its caller — it returns a predictable result even when the API misbehaves.

**Do this:** create Script Include **`WeatherGateway`** wrapping **RESTMessageV2**, calling the keyless [Open-Meteo API](https://open-meteo.com/). Handle a non-200 status, a timeout, and a body you cannot parse.

```javascript
var WeatherGateway = Class.create();
WeatherGateway.prototype = {

    initialize: function () {},

    // Returns { ok: true, tempC: <number> } or { ok: false, reason: <string> }.
    getCurrentTempC: function (lat, lon) {
        try {
            var r = new sn_ws.RESTMessageV2();
            r.setHttpMethod('GET');
            r.setEndpoint('https://api.open-meteo.com/v1/forecast?latitude=' +
                encodeURIComponent(lat) + '&longitude=' + encodeURIComponent(lon) +
                '&current=temperature_2m');
            r.setHttpTimeout(5000); // milliseconds

            var resp = r.execute();
            var status = resp.getStatusCode();
            if (status != 200) {
                return { ok: false, reason: 'HTTP ' + status };
            }

            var body = resp.getBody();
            var data;
            try {
                data = JSON.parse(body);
            } catch (parseErr) {
                return { ok: false, reason: 'Unparseable body' };
            }

            if (!data || !data.current || typeof data.current.temperature_2m === 'undefined') {
                return { ok: false, reason: 'Missing temperature in response' };
            }
            return { ok: true, tempC: Number(data.current.temperature_2m) };

        } catch (e) {
            // Network failure, timeout, DNS error, etc. — never leak a stack trace upward.
            return { ok: false, reason: 'Gateway exception: ' + e.message };
        }
    },

    type: 'WeatherGateway'
};
```

{% hint style="warning" %}
**Three things beginners get wrong here:**
1. `setHttpTimeout()` is in **milliseconds** — `5000` is 5 seconds, not 5000 seconds.
2. In a scoped app you must use the **`sn_ws.`** prefix: `new sn_ws.RESTMessageV2()`.
3. A gateway **returns a result object, it does not throw**. Every failure mode becomes `{ ok: false, reason: ... }`. That is what lets the rest of the app stay calm on a bad day.
{% endhint %}

**Test it** in Scripts - Background (London ≈ 51.51, -0.13):

```javascript
var gw = new x_course_lab.WeatherGateway();
gs.info('Good call:   ' + JSON.stringify(gw.getCurrentTempC(51.51, -0.13)));
gs.info('Broken call: ' + JSON.stringify(gw.getCurrentTempC('not', 'valid')));
```

**Done when:** the good call returns `{ ok: true, tempC: <number> }` and the broken call returns `{ ok: false, reason: ... }` — **not** a stack trace.

---

## Stage 4 · Service layer with dependency injection (~60 min)

**Goal:** the brain. It orchestrates the gateway and repository and owns **every** priority rule. Crucially, its dependencies are *injected* so you can test it with a fake gateway.

**Do this:** create Script Include **`FacilitiesService`**. Its `initialize(repository, gateway)` accepts both dependencies and falls back to the real ones when none are passed.

```javascript
var FacilitiesService = Class.create();
FacilitiesService.prototype = {

    // Dependencies are injected. Defaults are the real implementations.
    initialize: function (repository, gateway) {
        this.repo = repository || new x_course_lab.FacilitiesRepository();  // YOUR scope
        this.gateway = gateway   || new x_course_lab.WeatherGateway();
    },

    // Enriches the record with temperature and sets priority.
    // Returns { ok: true, tempC, priority } or { ok: false, reason }.
    enrichAndPrioritise: function (sysId, lat, lon) {
        var record = this.repo.getById(sysId);
        if (!record) return { ok: false, reason: 'Record not found' };

        var weather = this.gateway.getCurrentTempC(lat, lon);
        if (!weather.ok) return { ok: false, reason: 'Weather unavailable: ' + weather.reason };

        this.repo.updateTemp(sysId, weather.tempC);
        var priority = this._computePriority(record.request_type, weather.tempC);

        // Persist priority through the repository (add an updatePriority method,
        // or extend updateTemp — kept simple here for the walkthrough):
        var gr = new GlideRecordSecure(this.repo.TABLE);
        if (gr.get(sysId)) { gr.setValue('priority', priority); gr.update(); }

        return { ok: true, tempC: weather.tempC, priority: priority };
    },

    // ALL business rules live here and nowhere else. 1 = Critical ... 4 = Low.
    _computePriority: function (requestType, tempC) {
        if (requestType === 'cooling' && tempC > 30) return 1; // hot + wants cooling
        if (requestType === 'heating' && tempC < 5)  return 1; // cold + wants heating
        if (requestType === 'cooling' && tempC > 25) return 2;
        if (requestType === 'heating' && tempC < 10) return 2;
        return 3;
    },

    type: 'FacilitiesService'
};
```

**Test it — twice.** This double test is the entire point of dependency injection:

```javascript
// 1) Real dependencies (needs a real record + working API):
var repo = new x_course_lab.FacilitiesRepository();
var id = repo.create({ location: 'London HQ', request_type: 'cooling', short_description: 'Boiling' });
var svcReal = new x_course_lab.FacilitiesService();          // real gateway + repo
gs.info('Real:  ' + JSON.stringify(svcReal.enrichAndPrioritise(id, 51.51, -0.13)));

// 2) FAKE gateway — no network at all, forced 32°C:
var fakeGateway = { getCurrentTempC: function () { return { ok: true, tempC: 32 }; } };
var svcFake = new x_course_lab.FacilitiesService(repo, fakeGateway);   // injected!
gs.info('Fake:  ' + JSON.stringify(svcFake.enrichAndPrioritise(id, 0, 0)));
```

{% hint style="info" %}
**Why the fake test proves your architecture.** In the second run, no HTTP call happens — you supplied a substitute gateway. A `cooling` request at a forced 32 °C must come back `priority: 1`. If you cannot swap the gateway without editing `FacilitiesService`, your dependencies are **not** injected — revisit [Module 5.2](../module-5-architecture/01-dependency-injection-1.md).
{% endhint %}

**Done when:** both runs return `{ ok: true, ... }`, and the fake run returns `priority: 1` with no network call.

---

## Stage 5 · Controller + GlideAjax (~40 min)

**Goal:** the thin bridge between the browser and your service. It reads parameters, delegates to the service, and returns a **string** to the client.

{% hint style="danger" %}
**The one trap that breaks most submissions.** `AbstractAjaxProcessor` **already defines `initialize()`** — it wires up `getParameter()` from the incoming request. If you add your *own* `initialize(repository, gateway)` here (like you did in the service), you will overwrite the parent's and `getParameter('sysparm_location')` will return `undefined`.
**Rule: keep dependency injection in the *service*. The controller extends `AbstractAjaxProcessor` and does NOT declare its own `initialize`.**
{% endhint %}

**Do this:** create a Script Include **`FacilitiesController`**, tick **Client callable**, and extend `AbstractAjaxProcessor`.

```javascript
var FacilitiesController = Class.create();
FacilitiesController.prototype = Object.extendsObject(AbstractAjaxProcessor, {

    // NOTE: no custom initialize() — AbstractAjaxProcessor provides it.

    // Client-callable. MUST return a string (GlideAjax cannot return objects).
    validateLocation: function () {
        var location = this.getParameter('sysparm_location');

        // Server-side validation — never trust the client.
        var known = { 'London HQ': true, 'Bangkok Office': true, 'NYC Office': true };
        var result = {
            valid: !!(location && known[location]),
            message: (location && known[location]) ?
                'Location recognised.' :
                'Unknown location — a facilities agent will confirm the site.'
        };

        return JSON.stringify(result);   // <-- stringify before returning
    },

    type: 'FacilitiesController'
});
```

{% hint style="warning" %}
**GlideAjax returns strings only.** A client-callable method can return a string (or set the XML answer), never a JavaScript object or number. Always `JSON.stringify()` on the way out and `JSON.parse()` in the client callback.
{% endhint %}

**Test it** from the browser console on any form page (press F12):

```javascript
var ga = new GlideAjax('x_course_lab.FacilitiesController');  // YOUR scope
ga.addParam('sysparm_name', 'validateLocation');
ga.addParam('sysparm_location', 'London HQ');
ga.getXMLAnswer(function (answer) { console.log(JSON.parse(answer)); });
```

**Done when:** a known location logs `valid: true` and an unknown one logs `valid: false` with the guidance message.

---

## Stage 6 · User interface (~40 min)

**Goal:** a catalog item that guides the user as they type — without freezing the form.

**Do this:**

1. Create a **Catalog Item** ("Facilities Request") in a catalog/category you can reach in Service Portal.
2. Add variables: **Location** (single-line text, name `location`) and **Request type** (choice, name `request_type`, matching your field choices).
3. Add an **`onChange` catalog client script** on the `location` variable that calls the controller **asynchronously** and warns on an unknown location:

```javascript
function onChange(control, oldValue, newValue, isLoading) {
    if (isLoading || newValue === '') return;

    var ga = new GlideAjax('x_course_lab.FacilitiesController');  // YOUR scope
    ga.addParam('sysparm_name', 'validateLocation');
    ga.addParam('sysparm_location', newValue);
    ga.getXMLAnswer(function (answer) {
        var res = JSON.parse(answer);
        if (!res.valid) {
            g_form.showFieldMsg('location', res.message, 'warning');
        } else {
            g_form.hideFieldMsg('location', true);
        }
    });
}
```

{% hint style="warning" %}
**Keep the client lean.** Use `getXMLAnswer` (asynchronous) with a callback — never `getXMLWait()`. A synchronous call freezes the form while it waits on the server, which is an instant loss on the "Client-side quality & UX" criterion.
{% endhint %}

**Done when:** typing an unknown location shows a warning under the field before submit, and a known one clears it — with no perceptible form lag. Submitting creates a record in your table (via the record producer/flow behind the catalog item).

---

## Stage 7 · Automation (~20 min)

**Goal:** enrichment happens automatically on insert, with the Business Rule doing nothing but orchestration.

**Do this:** create an **after-insert Business Rule** on `x_course_lab_facilities_request`. Keep it to three or four lines — instantiate the service, call it, done. **No `if` statements about business meaning** live here; those belong in the service.

```javascript
(function executeRule(current, previous /*null when async*/) {

    // London coords stand in for a real location lookup you could add later.
    var svc = new x_course_lab.FacilitiesService();   // YOUR scope
    svc.enrichAndPrioritise(current.getUniqueValue(), 51.51, -0.13);

})(current, previous);
```

{% hint style="info" %}
**Async is the better answer.** Setting the Business Rule to run **after / async** means the user's submit does not wait on a third-party API. That is also stretch-goal territory (see below). For the core exercise, `after insert` is acceptable.
{% endhint %}

**Done when:** submitting the catalog item results — with no manual steps — in a record whose `outside_temp_c` is populated and whose `priority` matches your rules.

---

## Stage 8 · Cross-cutting concerns (~30 min)

**Goal:** one failure = one clear, actionable log line, and a table only the right people can touch.

**Do this:**

1. Create a **`GlobalErrorHandler`** Script Include (from [Module 6.5](../module-6-integrations/04-global-error-handling.md)) with a single logging entry point:

```javascript
var GlobalErrorHandler = Class.create();
GlobalErrorHandler.prototype = {
    initialize: function () {},

    // One consistent, greppable error line for every layer.
    log: function (layer, method, sysId, message) {
        gs.error('[FacilitiesLab] ' + layer + '.' + method +
            ' | record=' + (sysId || 'n/a') + ' | ' + message);
    },

    type: 'GlobalErrorHandler'
};
```

2. Route every `catch` in the gateway, repository, and service through it, e.g. in the gateway:

```javascript
} catch (e) {
    new x_course_lab.GlobalErrorHandler().log('WeatherGateway', 'getCurrentTempC', null, e.message);
    return { ok: false, reason: 'Gateway exception' };
}
```

3. Apply **least-privilege ACLs** on `x_course_lab_facilities_request` — scope create/read/write to the roles that need them rather than leaving table defaults wide open.

**Test it:** deliberately break the endpoint (change the URL in `WeatherGateway`), submit a request, then check the logs.

**Done when:** you break the weather API URL, submit a request, and find **exactly one** clear log entry naming the layer, method, and record — while the request record is **still created** (the failure degrades gracefully, it does not block intake).

---

## Stretch goals (optional, worth bonus points)

- [ ] Move enrichment to an **asynchronous** event or scheduled job ([Module 5.5](../module-5-architecture/04-queueing-event-driven.md)) so the user never waits on a third party.
- [ ] Add **retry with backoff** in the gateway for transient failures.
- [ ] **Cache** temperature per location for 15 minutes to avoid hammering the API.
- [ ] Call the service from a **Flow Designer custom action** ([Module 6.4](../module-6-integrations/03-flow-designer-custom-action.md)).
- [ ] Write a small **`FacilitiesServiceTest`** Script Include that runs your fake-dependency assertions and logs pass/fail.

## Deliverables

- [ ] Working scoped application in your PDI
- [ ] Exported scoped app or update set (XML)
- [ ] A short write-up (half a page) explaining your layers and one trade-off you made
- [ ] A 2–4 minute screen recording demoing the flow end to end
- [ ] Your completed [rubric](01-scoring-rubric.md) self-score
- [ ] Committed to your own GitHub repository as a portfolio piece

## Frequently asked questions

**Why does `getParameter('sysparm_location')` return `undefined` in my controller?**
You almost certainly added a custom `initialize()` to a Script Include that extends `AbstractAjaxProcessor`, which overwrote the parent one that wires up request parameters. Remove your custom `initialize` from the controller — dependency injection belongs in the service, not the client-callable class (see Stage 5).

**My GlideAjax callback logs `[object Object]` or nothing useful — why?**
GlideAjax can only carry a string back to the browser. `JSON.stringify()` your object in the controller and `JSON.parse()` it in the client callback (Stages 5–6).

**The weather call fails only in my scoped app but works in global — why?**
In a scoped application you must use the `sn_ws.` prefix: `new sn_ws.RESTMessageV2()`. Also confirm `setHttpTimeout()` is in milliseconds and your PDI has outbound internet access (Stage 3).

## Discussion and questions

Have a question or want to share your progress? Post a comment under the course videos and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/channel/UCMf7pje1x5iZkkEF-Y3PLSA)

## Resources

- [Module 5 · Architecture & Design Patterns](../module-5-architecture/README.md)
- [Module 6 · Integrations, Security & Reliability](../module-6-integrations/README.md)
- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [Open-Meteo API (no key required)](https://open-meteo.com/)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← How to Implement Global Error Handling](../module-6-integrations/04-global-error-handling.md) · [Scoring Rubric →](01-scoring-rubric.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
