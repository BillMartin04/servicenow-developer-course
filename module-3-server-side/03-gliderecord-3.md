---
description: "Part 3 explores advanced querying: OR conditions, ordering, aggregation, and dot-walking."
---

# GlideRecord Practical Demo — Part 3

**Quick answer:** Part 3 explores advanced querying: OR conditions, ordering, aggregation, and dot-walking. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=VJas9q5pNYU" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/VJas9q5pNYU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to use OR queries, ordering, aggregation, and dot-walking.

## Overview

Part 3 explores advanced querying: OR conditions, ordering, aggregation, and dot-walking.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=VJas9q5pNYU

## Key concepts

- **orWhere and query chaining** — `addQuery('priority', 1).addOrCondition('priority', 2)` groups conditions so records matching EITHER value pass, letting you build OR logic within what would otherwise be an AND chain.
- **orderBy** — `orderBy('priority')` and `orderByDesc('sys_created_on')` sort the result set server-side before you loop with `next()`, avoiding manual sorting in script.
- **GlideAggregate basics** — `GlideAggregate` is a separate class from `GlideRecord` used for COUNT, SUM, AVG, MIN, and MAX; you call `addAggregate('COUNT')` or similar, then `query()` and `next()`, and read results with `getAggregate('COUNT')` instead of field values.
- **Dot-walking to related records** — writing `gr.caller_id.email` follows a reference field to the related table's record and reads a field on it directly in script, without a second explicit query, though each dot-walk still costs a database lookup.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Count incidents per category and dot-walk to the caller's email for the newest one.

- [ ] In Scripts - Background, create a `GlideAggregate('incident')`
- [ ] Call `addAggregate('COUNT')` and `groupBy('category')`, then `query()`
- [ ] Loop with `while (ga.next())` and log each category with `getAggregate('COUNT')`
- [ ] Separately, create a `GlideRecord('incident')`, add `orderByDesc('sys_created_on')`, and `setLimit(1)`
- [ ] After `query()` and `next()`, dot-walk with `gr.caller_id.email` and log the value

**Done when:** the log shows a count per category that matches the numbers you see in an Incident list report, and the final log line shows a real email address (or blank if the caller field is empty) for the most recently created incident.

## Frequently asked questions

### Why do I need addOrCondition instead of just calling addQuery twice?

Calling `addQuery()` twice for the same field ANDs the conditions together, which usually returns zero results since a field can't equal two different values at once. `addOrCondition()` chains onto the previous query object to say "this OR that," which is what you actually want for something like priority 1 OR priority 2.

### Can I use GlideRecord for a simple count instead of GlideAggregate?

You technically can query and check `getRowCount()`, but that pulls the query's row count metadata without the flexibility of grouping or other aggregate functions. `GlideAggregate` is purpose-built for COUNT/SUM/AVG/MIN/MAX and is significantly cheaper on large tables since it doesn't need to instantiate full field data for every matching record.

### Is dot-walking the same as running a second query?

Dot-walking (`gr.caller_id.name`) looks like a simple property read but does trigger a lookup against the related table the first time you access that field, similar in cost to a separate query. Avoid dot-walking inside a loop over many records without care, since repeating it for each row can add up to a lot of extra database round-trips.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=VJas9q5pNYU)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/@techtalkwithbill)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← GlideRecord Practical Demo — Part 2](02-gliderecord-2.md) · [GlideRecord Practical Demo — Part 4 →](04-gliderecord-4.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
