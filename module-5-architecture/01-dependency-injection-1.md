---
description: "Dependency injection makes your ServiceNow code testable and loosely coupled."
---

# How to Implement Dependency Injection — Part 1

**Quick answer:** Dependency injection makes your ServiceNow code testable and loosely coupled. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=ypBiE0X2ATY" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/ypBiE0X2ATY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to decouple dependencies with constructor injection.

## Overview

Dependency injection makes your ServiceNow code testable and loosely coupled.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=ypBiE0X2ATY

## Key concepts

- **What DI is and why it matters** — instead of a class creating its own collaborators with `new` deep inside its methods, the collaborator is passed in from outside, so the class doesn't need to know how to construct it or which concrete implementation it's using.
- **Constructor injection in Script Includes** — `initialize(dependency)` accepts the dependency as a parameter and defaults to a real implementation when nothing is passed, e.g. `this.repo = dependency || new IncidentRepository();`, so existing callers keep working unchanged.
- **Decoupling dependencies** — the class only relies on the dependency's public methods (its shape), not on how it's built, which means the dependency can be swapped for a different real implementation or a fake without touching the class's own logic.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Build a Script Include that accepts its dependency through `initialize()` with a real default.

- [ ] Create a Script Include `IncidentNotifier` with `initialize(repository)` that sets `this.repository = repository || new IncidentRepository();`
- [ ] Implement `IncidentRepository` with a method `getActiveCount()` that returns a `GlideAggregate` count of active incidents
- [ ] Add a method `getSummary()` on `IncidentNotifier` that calls `this.repository.getActiveCount()` and returns a formatted string
- [ ] From Scripts - Background, call `new IncidentNotifier().getSummary()` with no argument so it uses the real default
- [ ] Call `new IncidentNotifier(new IncidentRepository())` explicitly and confirm the result matches

**Done when:** both calls return the same active incident count, proving the default dependency behaves identically to an explicitly passed one.

## Frequently asked questions

### Why default the dependency to a real implementation instead of requiring it every time?

Defaulting with `this.repo = dependency || new IncidentRepository();` means existing callers that just do `new IncidentNotifier()` keep working exactly as before, while tests or advanced callers can still pass in a specific instance. It avoids a breaking change when you introduce DI into an existing class.

### Isn't this just adding an extra parameter for no real benefit?

The benefit shows up in testing and reuse: once the dependency is injected rather than hardcoded, you can substitute a fake object in a test that returns a known value instead of hitting the real `GlideAggregate` query. Without injection, you're stuck testing against live data every time.

### Does dependency injection require a special ServiceNow framework or plugin?

No. This pattern is plain JavaScript using the existing `Class.create()` and `initialize()` mechanism — no additional plugin or framework is needed. It's a coding convention, not a platform feature.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=ypBiE0X2ATY)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/@techtalkwithbill)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← Object-Oriented Principles for Architects](00-oop-principles.md) · [Dependency Injection — Part 2 →](02-dependency-injection-2.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
