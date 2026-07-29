---
description: "Part 4 covers performance, best practices, and common anti-patterns to avoid in production."
---

# GlideRecord Practical Demo — Part 4

**Quick answer:** Part 4 covers performance, best practices, and common anti-patterns to avoid in production. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=zMMW_AdWrjI" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/zMMW_AdWrjI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to optimise queries and avoid performance anti-patterns.

## Overview

Part 4 covers performance, best practices, and common anti-patterns to avoid in production.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=zMMW_AdWrjI

## Key concepts

- **Query performance and indexes** — queries filtering on indexed fields (like `sys_id` or `number`) return far faster than queries on unindexed text fields, so check **System Definition > Indexes** before assuming a slow query is a scripting problem.
- **setWorkflow and autoSysFields** — `setWorkflow(false)` skips Business Rules and workflows for that write, and `autoSysFields(false)` stops the system from updating `sys_updated_on`/`sys_updated_by`, both useful for bulk data loads where you don't want side effects or audit noise.
- **Avoiding nested queries** — running a `GlideRecord` query inside a `while (gr.next())` loop of another query multiplies database round-trips and can turn a fast script into a slow one; dot-walking or a single combined query is almost always better.
- **Bulk operations** — always add `setLimit(n)` while developing a new bulk update or delete script so a logic mistake doesn't touch every row on the table before you've verified it on a small sample.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Refactor a naive nested-query script into a safer, faster version.

- [ ] In Scripts - Background, write a `GlideRecord('incident')` query that loops with `next()` and, inside the loop, runs a second `GlideRecord` query against `sys_user` for the caller
- [ ] Rewrite it to dot-walk (`gr.caller_id.name`) instead of the nested query, and compare log output for correctness
- [ ] Add `setLimit(5)` while testing so you don't process the whole table
- [ ] Add `gr.setWorkflow(false)` before a test `update()` call and confirm in **Business Rule** logs that no rule fired for that write
- [ ] Add `gr.autoSysFields(false)` before the same update and confirm `sys_updated_on` did not change

**Done when:** the dot-walked version produces the same caller names as the nested-query version, and you've confirmed both `setWorkflow(false)` and `autoSysFields(false)` suppressed their respective side effects.

## Frequently asked questions

### Does setWorkflow(false) stop ALL automation on my write?

It suppresses Business Rules and workflow/flow triggers tied to that specific `insert()` or `update()` call, but it does not bypass ACLs or database constraints. Use it deliberately for bulk data loads where re-running the full automation stack for every row would be slow or cause unwanted side effects like duplicate notifications.

### Why is my query slow even though the table isn't that big?

A query filtering on a field without a database index forces a full table scan instead of an indexed lookup, and this gets worse as related tables grow too. Check **System Definition > Indexes**, and also verify you're not running a `GlideRecord` query inside a loop over another query's results, since nested queries multiply the cost.

### When should I use setLimit() versus just trusting my query conditions?

Use `setLimit()` any time you're developing or testing a new query, especially one that writes or deletes, so a bug in your conditions can't accidentally process the entire table before you notice. Once the logic is verified on a small sample, you can remove the limit or raise it for production use.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=zMMW_AdWrjI)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/channel/UCMf7pje1x5iZkkEF-Y3PLSA)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← GlideRecord Practical Demo — Part 3](03-gliderecord-3.md) · [How Business Rules Execute (with Examples) →](05-business-rules-execution.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
