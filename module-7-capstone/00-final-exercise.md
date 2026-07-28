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
      │  injected ↓
FacilitiesService         business logic: orchestration + priority rules
      │  injected ↓
      ├── WeatherGateway      RESTMessageV2 wrapper for the weather API
      └── FacilitiesRepository GlideRecordSecure read/write
                │
        GlobalErrorHandler    used by every layer
```

Each layer has exactly one job. The controller talks to the client, the service decides, the gateway talks outward, the repository talks to the database, and the error handler makes failures visible. This mirrors [Module 5 · Architecture](../module-5-architecture/README.md) and [Module 6 · Integrations](../module-6-integrations/README.md).

## Build it in stages

Work through the stages in order. Each stage should be working before you start the next.

### Stage 1 · Data model (~30 min)

In your `x_course_lab` scoped application:

- Create table `x_course_lab_facilities_request`, **extending Task**.
- Add fields: `location` (String), `request_type` (Choice: heating / cooling / lighting / other), `outside_temp_c` (Decimal).
- `priority`, `state`, `short_description`, and `assigned_to` come from Task — do not recreate them.

**Done when:** you can insert a record from the list view and see it in the table.

### Stage 2 · Repository layer (~40 min)

- Create Script Include `FacilitiesRepository`.
- Methods: `create(payload)`, `getById(sysId)`, `updateTemp(sysId, tempC)`.
- Use **GlideRecordSecure** and `getValue()` / `setValue()` — never dot-walk into a field and hope.
- **No business logic in here.** This layer only reads and writes.

**Done when:** you can create and read a record entirely through the repository from Scripts - Background.

### Stage 3 · Gateway layer (~50 min)

- Create Script Include `WeatherGateway` wrapping **RESTMessageV2**.
- Method: `getCurrentTempC(lat, lon)` calling a keyless public weather API (Open-Meteo works well).
- Handle a non-200 status, a timeout, and a malformed body — each must return a predictable result rather than throwing raw.

**Done when:** calling it with valid coordinates returns a number, and calling it with a broken URL returns a controlled failure, not a stack trace.

### Stage 4 · Service layer with dependency injection (~60 min)

- Create Script Include `FacilitiesService`.
- Its `initialize(repository, gateway)` **accepts both dependencies**, defaulting to the real ones when none are passed.
- Method `enrichAndPrioritise(sysId, lat, lon)`:
  1. gets the temperature from the gateway,
  2. persists it through the repository,
  3. computes priority — for example `cooling` + temp > 30 °C → high; `heating` + temp < 5 °C → high.
- All priority rules live **here**, nowhere else.

**Done when:** you can run the service in Scripts - Background twice — once with the real gateway, once with a fake gateway object that returns a fixed temperature — and get correct priorities both times.

{% hint style="info" %}
That fake-gateway test is the whole point of dependency injection. If you cannot substitute the gateway, your dependencies are not injected — go back to [Module 5.2](../module-5-architecture/01-dependency-injection-1.md).
{% endhint %}

### Stage 5 · Controller + GlideAjax (~40 min)

- Create `FacilitiesController`, client-callable, extending `AbstractAjaxProcessor`.
- Expose `validateLocation()` which reads `sysparm_location`, validates it server-side, and returns a result.
- The controller delegates to the service — it must contain no business rules of its own.

**Done when:** you can call it from a browser console via GlideAjax and get a sensible answer for both a valid and an invalid location.

### Stage 6 · User interface (~40 min)

- Build a **catalog item** with variables for location and request type.
- Add a **catalog client script** that calls `validateLocation()` through **GlideAjax** as the user types or on change, and warns when the location is unknown.
- Keep the client script lean — no synchronous server calls, no heavy loops.

**Done when:** submitting the catalog item creates a record in your table, and an unknown location shows a warning before submit.

### Stage 7 · Automation (~20 min)

- Add an **after-insert Business Rule** (or a Flow action) that calls `FacilitiesService.enrichAndPrioritise()`.
- The rule should be **three or four lines**: instantiate the service, call the method, done.

**Done when:** submitting the catalog item results in a record with a populated temperature and a correctly computed priority, with no manual steps.

### Stage 8 · Cross-cutting concerns (~30 min)

- Route every `try/catch` in every layer through a `GlobalErrorHandler` Script Include (from [Module 6.5](../module-6-integrations/04-global-error-handling.md)).
- Every logged error must include: the layer, the method, the record sys_id, and the original message.
- Apply least-privilege **ACLs** on your table.

**Done when:** you can deliberately break the weather API URL, submit a request, and find one clear, useful log entry — while the record is still created.

## Stretch goals (optional, worth bonus points)

- [ ] Move the enrichment to an **asynchronous** pattern using an event or scheduled job ([Module 5.5](../module-5-architecture/04-queueing-event-driven.md)) so the user is never waiting on a third party.
- [ ] Add a **retry with backoff** in the gateway for transient failures.
- [ ] Cache temperature per location for 15 minutes to avoid hammering the API.
- [ ] Call the service from a **Flow Designer custom action** ([Module 6.4](../module-6-integrations/03-flow-designer-custom-action.md)).
- [ ] Write a small `FacilitiesServiceTest` Script Include that runs your fake-dependency assertions and logs pass/fail.

## Deliverables

- [ ] Working scoped application in your PDI
- [ ] Exported scoped app or update set (XML)
- [ ] A short write-up (half a page) explaining your layers and one trade-off you made
- [ ] A 2–4 minute screen recording demoing the flow end to end
- [ ] Your completed [rubric](01-scoring-rubric.md) self-score
- [ ] Committed to your own GitHub repository as a portfolio piece

## Next steps

1. **Score yourself** → [Scoring Rubric](01-scoring-rubric.md)
2. **Get it verified** → [Submit Your Work for Verification](02-submit-for-verification.md)

## Resources

- [Module 5 · Architecture & Design Patterns](../module-5-architecture/README.md)
- [Module 6 · Integrations, Security & Reliability](../module-6-integrations/README.md)
- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [Open-Meteo API (no key required)](https://open-meteo.com/)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

---
<!--NAV-->
[← How to Implement Global Error Handling](../module-6-integrations/04-global-error-handling.md) · [Scoring Rubric →](01-scoring-rubric.md)
