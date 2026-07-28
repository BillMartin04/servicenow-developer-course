---
description: >-
  Plain-English definitions of the ServiceNow and development terms used
  throughout this course — from PDI and scope to ACLs, GlideAjax, and
  dependency injection.
---

# Glossary

Every term below appears somewhere in the course. If a lesson uses a word you are not sure about, look it up here. Definitions are short and practical, not textbook.

{% hint style="info" %}
Terms are grouped by theme, then alphabetical within each group. Bold cross-references point to related entries.
{% endhint %}

## Platform & environment

**Instance** — A single running copy of ServiceNow (a URL like `dev12345.service-now.com`). Your work lives inside an instance.

**PDI (Personal Developer Instance)** — A free, personal ServiceNow instance from the [ServiceNow Developer Program](https://developer.servicenow.com). Every hands-on exercise in this course runs in your PDI. It hibernates after inactivity and can be reclaimed if unused, so wake it up before you work.

**Now Platform** — ServiceNow's underlying platform: the database, UI, workflow engine, and scripting runtime that all applications are built on.

**Update Set** — A named container that captures configuration changes so you can move them between instances (e.g. dev → test → prod). The classic way to package non-scoped changes.

**Application / Scoped App** — A packaged application with its own namespace (scope). Scoped apps isolate tables, scripts, and APIs from the `global` scope and from each other, which is safer and more portable.

**Scope** — The namespace an application's code runs in. **Global scope** is the shared, legacy space; a **scoped application** has a private namespace (e.g. `x_myco_app`). Scope affects which APIs you use — for example, scoped REST uses `sn_ws.RESTMessageV2()`.

## Scripting — server side

**GlideRecord** — The primary server-side API for querying, creating, updating, and deleting records. `getValue()` on a GlideRecord always returns a **string**.

**GlideRecordSecure** — Like GlideRecord, but it enforces **ACLs** (row- and field-level security) for the current user. Use it whenever a query is driven by user input.

**GlideSystem (`gs`)** — The server-side utility object: logging (`gs.info`), the current user (`gs.getUserID`), properties (`gs.getProperty`), events (`gs.eventQueue`), and more.

**Business Rule** — Server-side logic that runs when a record is queried, inserted, updated, or deleted. Runs **before**, **after**, **async**, or on **display** — the timing matters (see the [Cheat Sheet](cheat-sheet.md)).

**Script Include** — Reusable server-side JavaScript, defined once and called from many places. The building block of a clean service layer. Can be **client-callable** (via GlideAjax) when it extends **AbstractAjaxProcessor**.

**AbstractAjaxProcessor** — The base class a client-callable Script Include extends via `Object.extendsObject`. It supplies `getParameter()` and the request plumbing — which is why a client-callable Script Include **must not define its own `initialize()`**.

**Event / Event Queue** — A named signal you fire with `gs.eventQueue('name', current, p1, p2)` to trigger asynchronous work (like a notification or a Script Action) without blocking the current transaction.

**ES5 / ES-latest** — JavaScript versions. Server-side scripts default to **ES5** (use `var` and `function`, avoid arrow functions and `let`/`const`) unless the script runs in an ES-latest-enabled context.

## Scripting — client side

**Client Script** — JavaScript that runs in the browser on a form (onLoad, onChange, onSubmit, onCellEdit). Use it for UI behaviour, never for direct database access.

**g_form** — The client-side API for the current form: read/write fields (`getValue`, `setValue`), control visibility and mandatory/read-only state, and show messages.

**g_scratchpad** — A data object populated by a **display** Business Rule on the server and read by Client Scripts, so the client has server data without an extra call.

**UI Policy** — A no-code (or low-code) way to make fields mandatory, read-only, or hidden based on conditions. Prefer it over a Client Script when a UI Policy can do the job.

**GlideAjax** — The bridge that lets a Client Script call a server-side Script Include and get a value back. It **only ever returns a string** — `JSON.stringify()` on the server, `JSON.parse()` on the client. Prefer the async `getXMLAnswer(callback)`.

## Security & integration

**ACL (Access Control List)** — A rule that decides whether the current user can read, write, create, or delete a record or field. **GlideRecordSecure** and the platform UI honour ACLs; a plain **GlideRecord** does not.

**Role** — A named permission group (e.g. `admin`, `itil`) assigned to users/groups and referenced by ACLs. Check with `gs.hasRole('...')`.

**REST / REST API** — The standard way to send and receive data over HTTP with other systems. In scoped apps, outbound calls use **`sn_ws.RESTMessageV2()`**; `setHttpTimeout()` is in milliseconds.

**Connection & Credential Alias** — The secure, configurable way to store an endpoint URL and its credentials, so you **never hard-code credentials** in a script.

**Flow Designer** — ServiceNow's low-code automation tool for building flows (triggers + actions). You can extend it with a **custom action** backed by a Script Include.

## Architecture & design patterns

**OOP (Object-Oriented Programming)** — Organising code into objects/classes with clear responsibilities. In ServiceNow, `Class.create()` + `Object.extendsObject()` are the tools.

**Dependency Injection (DI)** — Passing a component's dependencies in from the outside (instead of it creating them internally), so code is loosely coupled and testable. In this course, DI lives in the **service layer**, not the GlideAjax controller.

**Service Layer** — The Script Include(s) that hold the actual business logic. Controllers (like GlideAjax processors) stay thin and delegate to the service layer — the separation that keeps code testable and safe for AI to extend.

**Controller** — A thin entry point (e.g. a client-callable Script Include) that reads parameters and delegates to the service layer. It should contain no business logic.

**Event-Driven** — A design where components react to events rather than calling each other directly, reducing coupling. ServiceNow events and the event queue support this.

**Service Bus** — An architectural pattern where a central component routes messages/requests between services, so producers and consumers do not depend on each other directly.

## AI-readiness

**AI-ready** — Code and architecture built so that AI coding assistants and platform agents can extend it **without breaking it** — clean layers, reusable Script Includes, injected dependencies, consistent error handling. It also means *you* have the judgment to review and correct what an AI produces. Without these foundations, the correctness of an AI build is a gamble.

**Review judgment** — The human skill this course builds: being able to look at generated (or copied) ServiceNow code and tell whether it is correct, safe, and maintainable. AI can produce code fast; only a trained developer can verify it.

## Continue the course

* **Back to:** [Welcome](../welcome.md)
* **Related:** [Cheat Sheet](cheat-sheet.md) · [Debugging Guide](debugging-guide.md)
