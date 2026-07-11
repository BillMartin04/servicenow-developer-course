# Capstone Project: Build a Production-Ready App

_Part of Module 7 · Capstone & Next Steps · [ServiceNow Developer Course](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)_

## Watch

{% hint style="info" %}
**Video to record.** The full project brief below is self-contained — you can build it now.
{% endhint %}

## Overview

Time to prove it. In this capstone you'll build a small but **production-quality** scoped application that uses every major pattern from the course: client and server scripting, Script Includes, GlideAjax, an external integration, dependency injection, secure coding, and global error handling. This is the kind of artefact you can demo in an interview or add to your portfolio.

## The scenario

> **"Weather-Aware Facilities Requests"**
> Your organisation lets staff raise facilities requests (e.g. "office too cold"). The business wants each request enriched with the current outside temperature for the office location, and wants the intake form to guide users intelligently. Managers must review high-impact requests.

You'll build a scoped app that:

1. Provides a **catalog item** for staff to submit a facilities request.
2. On the form, uses **client scripting + GlideAjax** to validate input and show live guidance.
3. On submit, **fetches current weather** from a public REST API and stores it on the record.
4. Applies **Business Rules** to set priority based on request type + temperature.
5. Wraps all server logic in **layered Script Includes** (repository → service) with **dependency injection**, **secure coding**, and **global error handling**.

## Architecture (the target)

```
Catalog Item (UI)
      │  (client script + GlideAjax)
      ▼
FacilitiesController  (client-callable Script Include)
      │  depends on ↓ (injected)
FacilitiesService     (business logic: priority, orchestration)
      │  depends on ↓ (injected)
WeatherGateway        (RESTMessageV2 wrapper for the weather API)
FacilitiesRepository  (GlideRecord read/write, GlideRecordSecure)
GlobalErrorHandler    (used everywhere)
```

This mirrors the [architecture module](../module-5-architecture/README.md): separation of concerns, testable units, no business logic buried in Business Rules.

## Build steps

### 1. Data model
- In your `x_course_lab` scope, create a table `x_course_lab_facilities_request` extending Task, with fields: `location` (string), `request_type` (choice: heating/cooling/lighting/other), `outside_temp_c` (decimal), `priority` (from Task).

### 2. Repository layer
- Create `FacilitiesRepository` Script Include. Methods: `create(record)`, `getById(sysId)`, `updateTemp(sysId, temp)`. Use **GlideRecordSecure** and `getValue()`.

### 3. Gateway layer
- Create `WeatherGateway` Script Include wrapping `RESTMessageV2` to call a public weather API (e.g. Open-Meteo, no key required). Method: `getCurrentTempC(lat, lon)`. Handle timeouts and non-200 responses.

### 4. Service layer with dependency injection
- Create `FacilitiesService`. Its `initialize()` accepts a repository and gateway (inject them; default to real ones if none passed). Method `enrichAndPrioritise(sysId, lat, lon)`:
  - calls the gateway for temperature,
  - stores it via the repository,
  - computes priority (e.g. cooling request + temp > 30°C → high).

### 5. Controller + GlideAjax
- Create `FacilitiesController` (client-callable, extends `AbstractAjaxProcessor`). Expose `validateLocation()` for the client script to call during input.

### 6. UI
- Build a **catalog item** with variables for location and request type.
- Add a **catalog client script** that uses **GlideAjax** to call `validateLocation()` and warn if the location is unknown.

### 7. Business Rule
- Add an **after** business rule (or call from a flow) that invokes `FacilitiesService.enrichAndPrioritise()` on insert.

### 8. Cross-cutting
- Route every `try/catch` through a `GlobalErrorHandler` Script Include (from [Module 6](../module-6-integrations/04-global-error-handling.md)) for consistent logging.

## Code review checklist

{% hint style="success" %}
Grade your own build against this before calling it done.
{% endhint %}

- [ ] No business logic inside Business Rules — they only orchestrate/call services
- [ ] GlideRecord access is confined to the repository layer
- [ ] External call is isolated in the gateway and handles failure gracefully
- [ ] Dependencies are injected, so the service is testable with fakes
- [ ] Client script never trusts unvalidated input; server re-validates
- [ ] Secure coding: GlideRecordSecure, input validation, least-privilege ACLs
- [ ] All errors flow through the global error handler with useful context
- [ ] Everything lives in one scoped app and exports cleanly

## Deliverable

- [ ] Working scoped app in your PDI
- [ ] A short screen recording or write-up demoing the flow (great portfolio piece)
- [ ] Export the update set / scoped app and commit it to your own GitHub for your portfolio

## Resources

- [Module 5 · Architecture](../module-5-architecture/README.md)
- [Module 6 · Integrations & Reliability](../module-6-integrations/README.md)
- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/@techtalkwithbill)
