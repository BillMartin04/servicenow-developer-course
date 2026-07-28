---
description: "Architect-level patterns for structuring Script Includes and GlideAjax at scale."
---

# Script Include & GlideAjax for Architects

**Quick answer:** Architect-level patterns for structuring Script Includes and GlideAjax at scale. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=8nYD0R-dYcA" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/8nYD0R-dYcA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to apply layering and the Repository pattern.

## Overview

Architect-level patterns for structuring Script Includes and GlideAjax at scale.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=8nYD0R-dYcA

## Key concepts

- **Layering and separation of concerns** — split the client-callable Script Include (controller), the business-logic class (service), and the data-access class (repository) into three separate classes so each layer has one job and can change independently.
- **The thin, client-callable controller** — the Script Include exposed to GlideAjax extends `AbstractAjaxProcessor`, must NOT declare a custom `initialize()`, and should only read request parameters with `getParameter()`, delegate to a service, and return a string result — no business logic lives here.
- **The Repository pattern** — a repository class wraps all `GlideRecord` access for a table behind methods like `getById()` or `getActiveCount()`, so the service layer works with plain data instead of query syntax, and the repository is the one place that changes if the table schema changes.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Build a three-layer GlideAjax stack: controller, service, and repository.

- [ ] Create `IncidentRepository` with a method `getActiveCount()` that runs a `GlideAggregate` count on the Incident table
- [ ] Create `IncidentService` with `initialize(repository)` defaulting to `new IncidentRepository()`, and a method `getActiveCountMessage()` that formats the repository's result as a string
- [ ] Create a client-callable Script Include `IncidentAjax` that extends `AbstractAjaxProcessor` (no custom `initialize()`) with a method `getActiveCountMessage()` that just calls `new IncidentService().getActiveCountMessage()` and returns it
- [ ] Call `IncidentAjax` from a client script or UI page using `GlideAjax`, and confirm the returned string matches what you get calling `IncidentService` directly in Scripts - Background

**Done when:** the GlideAjax call returns the same message as calling `IncidentService` directly, and `IncidentAjax` contains no query logic or custom `initialize()` of its own.

## Frequently asked questions

### Why can't the client-callable Script Include just do the GlideRecord query itself?

It technically can, but that mixes request handling with business logic and data access in one class, making it impossible to reuse the logic outside of GlideAjax (e.g. from a Scheduled Job) or to test it without simulating an Ajax call. Keeping the controller thin means the real logic lives in a service class that any caller can use.

### Why must the client-callable controller avoid a custom `initialize()`?

`AbstractAjaxProcessor` already defines its own `initialize()` that wires up the request and response objects for GlideAjax; overriding it would break that wiring. Any dependencies the controller needs should instead be constructed inside its methods by calling the service layer, which is where dependency injection belongs.

### What's the actual difference between the service layer and the repository layer?

The repository's only job is talking to the database — building `GlideRecord`/`GlideAggregate` queries and returning results. The service layer contains the business rules and formatting that act on that data, and it depends on the repository through injection rather than querying tables directly.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=8nYD0R-dYcA)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/@techtalkwithbill)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← Dependency Injection — Part 2](02-dependency-injection-2.md) · [Queueing & Event-Driven Architecture →](04-queueing-event-driven.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
