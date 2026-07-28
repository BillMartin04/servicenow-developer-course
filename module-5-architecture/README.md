# Module 5 · Architecture & Design Patterns

**Estimated time:** ~1 hr 15 min  ·  **Topics:** 6

Apply professional software architecture inside ServiceNow: OOP, dependency injection, event-driven processing, and a service-bus integration layer.

## Learning objectives

By the end of this module, you will be able to:

- Apply OOP and SOLID principles to ServiceNow scripting
- Implement dependency injection for testable, decoupled code
- Design event-driven and queued processing for scalability
- Structure reusable integrations with a service-bus pattern

## Prerequisites

Modules 0–4. Solid grasp of Script Includes and GlideAjax is essential.

## Topics

| # | Topic | Type | Time | You'll learn to |
| --- | --- | --- | --- | --- |
| 5.1 | [Object-Oriented Principles for Architects](00-oop-principles.md) | 🎬 Video | 14 min | Apply encapsulation, SOLID, and cohesion in ServiceNow |
| 5.2 | [How to Implement Dependency Injection — Part 1](01-dependency-injection-1.md) | 🎬 Video | 12 min | Decouple dependencies with constructor injection |
| 5.3 | [Dependency Injection — Part 2](02-dependency-injection-2.md) | 🎬 Video | 12 min | Build a simple service container pattern |
| 5.4 | [Script Include & GlideAjax for Architects](03-script-include-glideajax-arch.md) | 🎬 Video | 13 min | Apply layering and the Repository pattern |
| 5.5 | [Queueing & Event-Driven Architecture](04-queueing-event-driven.md) | 🎬 Video | 13 min | Decouple work with events and async processing |
| 5.6 | [Service Bus Architecture for Reusable Integrations](05-service-bus.md) | 🎬 Video | 12 min | Design a scalable, reusable integration layer |

{% hint style="info" %}
**Why layering comes before async:** you need clean layers before you can safely decouple them with events, and before a service bus makes any sense.
{% endhint %}

## Knowledge check

Before moving on, make sure you can answer these:

- How does dependency injection make code testable?
- When would you fire an event instead of doing the work inline?
- What responsibility does a repository class isolate?

## Module recap

{% hint style="success" %}
You can architect maintainable, testable, scalable solutions instead of scattering logic across Business Rules.
{% endhint %}

## Need help?

Ask your question in the comments of the video for the topic you are stuck on — see [Get Help & Get Verified](../support-and-verification.md).

**Next up →** [Module 6 · Integrations, Security & Reliability](../module-6-integrations/README.md)
