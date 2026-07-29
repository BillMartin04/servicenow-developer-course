---
description: "GlideRecord is the core server-side API for querying and manipulating records."
---

# GlideRecord Practical Demo — Part 1

**Quick answer:** GlideRecord is the core server-side API for querying and manipulating records. Part 1 covers query, next, and get. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=FAfwLYSrqO0" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/FAfwLYSrqO0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to query records with addQuery, next, and get.

## Overview

GlideRecord is the core server-side API for querying and manipulating records. Part 1 covers query, next, and get.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=FAfwLYSrqO0

## Key concepts

- **GlideRecord query lifecycle** — you build a `GlideRecord` object, add conditions, call `query()` to execute, then loop with `next()`; the query only hits the database once you call `query()`.
- **addQuery / addEncodedQuery** — `addQuery('priority', 1)` adds a single AND condition; `addEncodedQuery('priority=1^active=true')` applies a full encoded query string copied straight from a filter's breadcrumb.
- **next() iteration** — after `query()`, `while (gr.next()) { ... }` advances the cursor one matching record at a time; each iteration exposes that record's fields on `gr`.
- **get() by sys_id** — `gr.get(sysId)` is a shortcut that queries, calls `next()`, and returns `true`/`false` in one step, but it also accepts a field/value pair like `gr.get('number', 'INC0010001')`.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Query the Incident table for all active P1 incidents and log their numbers.

- [ ] In Scripts - Background, create a `GlideRecord('incident')`
- [ ] Add `addQuery('priority', 1)` and `addQuery('active', true)`
- [ ] Call `query()`, then loop with `while (gr.next())`
- [ ] Log `gr.getValue('number')` for each match with `gs.info()`
- [ ] Pick one returned sys_id and confirm `gr.get(sysId)` returns the same record

**Done when:** the log lists the numbers of every active P1 incident, and re-running after closing one shows one fewer result.

## Frequently asked questions

### When should I use addQuery vs addEncodedQuery?

Use `addQuery()` when building conditions programmatically field by field, since it's readable and safe from typos in operators. Use `addEncodedQuery()` when you've copied a query string directly from a list's breadcrumb or filter, especially for complex OR logic, but always validate encoded strings from user input since they aren't automatically sanitized like chained `addQuery()` calls.

### Why does gr.getValue('number') return a string instead of a number?

`GlideRecord` fields are wrapped in a `GlideElement`, and `getValue()` always returns the underlying database value as a plain string, even for integer or numeric fields. If you need to do arithmetic, explicitly convert with `parseInt()` or `parseFloat()` first.

### What's the difference between query() and get()?

`query()` executes whatever conditions you've added and requires a `while (gr.next())` loop to step through possibly many results. `get()` is built for the common case of fetching exactly one record by sys_id (or a unique field), combining the query, the first `next()`, and a boolean success check into a single call.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=FAfwLYSrqO0)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/channel/UCMf7pje1x5iZkkEF-Y3PLSA)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← Understanding GlideSystem for Efficient Scripting](00-glidesystem.md) · [GlideRecord Practical Demo — Part 2 →](02-gliderecord-2.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
