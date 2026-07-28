# Module 3 · Server-Side Scripting

**Estimated time:** ~1 hr 20 min  ·  **Topics:** 7

Work on the server with GlideSystem, Business Rules, and the GlideRecord API — the core of reading and writing data in ServiceNow.

## Learning objectives

By the end of this module, you will be able to:

- Use GlideSystem (gs) for logging, users, dates, and properties
- Write before, after, async, and display Business Rules correctly
- Query, insert, update, and delete records with GlideRecord
- Apply GlideRecord performance best practices and avoid anti-patterns

## Prerequisites

Modules 0–2. Comfort with server-side execution and JavaScript basics.

## Topics

| # | Topic | Type | Time | You'll learn to |
| --- | --- | --- | --- | --- |
| 3.1 | [Understanding GlideSystem for Efficient Scripting](00-glidesystem.md) | 🎬 Video | 11 min | Use gs for logging, users, dates, and properties |
| 3.2 | [GlideRecord Practical Demo — Part 1](01-gliderecord-1.md) | 🎬 Video | 12 min | Query records with addQuery, next, and get |
| 3.3 | [GlideRecord Practical Demo — Part 2](02-gliderecord-2.md) | 🎬 Video | 12 min | Insert, update, and delete records safely |
| 3.4 | [GlideRecord Practical Demo — Part 3](03-gliderecord-3.md) | 🎬 Video | 12 min | Use OR queries, ordering, aggregation, and dot-walking |
| 3.5 | [GlideRecord Practical Demo — Part 4](04-gliderecord-4.md) | 🎬 Video | 12 min | Optimise queries and avoid performance anti-patterns |
| 3.6 | [How Business Rules Execute (with Examples)](05-business-rules-execution.md) | 🎬 Video | 13 min | Master the four Business Rule timings and order of execution |
| 3.7 | [Display Business Rules & g_scratchpad](06-display-business-rules.md) | 🎬 Video | 9 min | Pass server data to the client with g_scratchpad |

{% hint style="info" %}
**Why GlideRecord comes before Business Rules:** a Business Rule script is mostly GlideRecord code. Learn the API first and the automation topic becomes obvious instead of overwhelming.
{% endhint %}

## Knowledge check

Before moving on, make sure you can answer these:

- What is the difference between a before and an after Business Rule?
- How do you pass server data to a client script before the form loads?
- Name two GlideRecord practices that improve query performance.

## Module recap

{% hint style="success" %}
You can read and write platform data reliably, run logic at the right Business Rule stage, and keep queries fast.
{% endhint %}

## Why this makes you AI-ready

{% hint style="info" %}
This is where generated code is most dangerous. An AI will write a plain `GlideRecord` query that ignores ACLs and leaks data, put logic in a Business Rule with the wrong `when` timing so it runs at the wrong moment, or query inside a loop and cripple performance — all while looking correct. Knowing GlideRecord, Business Rule timing, and security *by hand* is exactly what lets you review AI output line by line and know it's safe.
{% endhint %}

## Need help?

Ask your question in the comments of the video for the topic you are stuck on — see [Get Help & Get Verified](../support-and-verification.md).

**Next up →** [Module 4 · Script Includes & GlideAjax](../module-4-script-includes-glideajax/README.md)
