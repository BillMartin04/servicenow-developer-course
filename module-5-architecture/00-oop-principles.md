---
description: "Apply OOP principles — encapsulation, inheritance, polymorphism, SOLID — inside ServiceNow."
---

# Object-Oriented Principles for Architects

**Quick answer:** Apply OOP principles — encapsulation, inheritance, polymorphism, SOLID — inside ServiceNow. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=r9n5C74a-cE" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/r9n5C74a-cE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to apply encapsulation, SOLID, and cohesion in ServiceNow.

## Overview

Apply OOP principles — encapsulation, inheritance, polymorphism, SOLID — inside ServiceNow.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=r9n5C74a-cE

## Key concepts

- **Encapsulation and cohesion** — a Script Include should hide its internal fields behind methods and expose only what callers need; a cohesive class has one clear job, so unrelated logic (e.g. notification formatting inside a pricing calculator) belongs in a separate class.
- **SOLID in ServiceNow scripting** — the Single Responsibility Principle matters most day-to-day: if a Script Include's name needs "and" to describe it (e.g. `ValidateAndNotify`), it is doing two jobs and should be split so each class has one reason to change.
- **Designing maintainable Script Includes** — favour small classes with a narrow public API, low coupling (few hard dependencies on other classes) and high cohesion (related methods grouped together), so a future change only touches one file.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Refactor a procedural Script Include so it follows single responsibility and hides its internal state.

- [ ] Create a Script Include `IncidentPriorityCalculator` with a public method `calculatePriority(impact, urgency)`
- [ ] Move the priority lookup logic into a private helper method (prefix with `_`) instead of inlining it in `calculatePriority`
- [ ] Store any lookup table (e.g. impact/urgency matrix) as a variable set in `initialize()`, not hardcoded inside the method
- [ ] Call the class from Scripts - Background and log the returned priority for a few impact/urgency combinations

**Done when:** `calculatePriority()` returns the correct priority for each combination and none of the calling code needs to know how the lookup is implemented internally.

## Frequently asked questions

### What is the difference between cohesion and coupling?

Cohesion is about how focused a single class is — a highly cohesive Script Include does one job well. Coupling is about how much one class depends on the internals of another. You want high cohesion within classes and low coupling between them, so changing one Script Include rarely forces changes elsewhere.

### How does encapsulation actually work in a Script Include, since JavaScript has no `private` keyword?

ServiceNow's `Class.create()` pattern doesn't enforce true privacy, but the convention is to prefix internal helper methods and variables with an underscore (e.g. `_buildQuery`) and document them as "not part of the public API." Callers are expected to only use the non-underscore methods, even though nothing technically blocks them from calling the rest.

### Why does Single Responsibility matter if the script still works fine as one big class?

A single large class still "works" until two people need to change different behaviors at the same time, or a bug fix in one area breaks unrelated functionality. Splitting by responsibility means each class has one reason to change, which makes testing, code review, and reuse much easier as the app grows.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=r9n5C74a-cE)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/@techtalkwithbill)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← Script Include & Object.extendObject — Part 2](../module-4-script-includes-glideajax/03-extendobject-2.md) · [How to Implement Dependency Injection — Part 1 →](01-dependency-injection-1.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
