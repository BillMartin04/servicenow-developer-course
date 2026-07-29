---
description: "Part 2 covers inserting, updating, and deleting records safely with GlideRecord."
---

# GlideRecord Practical Demo — Part 2

**Quick answer:** Part 2 covers inserting, updating, and deleting records safely with GlideRecord. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=gAO5l6MQC2E" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/gAO5l6MQC2E" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to insert, update, and delete records safely.

## Overview

Part 2 covers inserting, updating, and deleting records safely with GlideRecord.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=gAO5l6MQC2E

## Key concepts

- **insert / update / deleteRecord** — `insert()` creates a new row and returns its sys_id, `update()` saves changes on a record loaded via `get()`/`next()`, and `deleteRecord()` removes the current record from the table.
- **setValue and setLimit** — `setValue('field', value)` stages a field change without immediately writing to the database, while `setLimit(n)` caps how many records a query returns, which is unrelated to writes but easy to confuse with them.
- **Write operations and ACLs** — every `insert()`, `update()`, and `deleteRecord()` call is still subject to the table's write ACLs for the running user, so a script can silently fail to save if the user lacks permission unless you check the return value or use `setForceUpdate(true)` deliberately.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Create, modify, and remove a test incident entirely through script.

- [ ] In Scripts - Background, create a `GlideRecord('incident')`, `setValue()` a short description, then call `insert()` and log the returned sys_id
- [ ] Use `get(sysId)` to load that record back, `setValue()` the priority to a new value, and call `update()`
- [ ] Query for the record again and `gs.info()` its priority to confirm the update saved
- [ ] Call `deleteRecord()` on that record
- [ ] Query for the sys_id again and confirm `get()` now returns `false`

**Done when:** you've logged the sys_id after insert, confirmed the updated priority value, and confirmed the record no longer exists after `deleteRecord()`.

## Frequently asked questions

### Why doesn't my update() call seem to save anything?

The most common cause is calling `setValue()` on a `GlideRecord` that was never loaded with `get()`, `next()`, or a completed `query()`, so there's no existing row to update. Also check the return value of `update()` and the System Log for ACL-related security warnings, since a failed write due to insufficient access often fails silently rather than throwing an error.

### Should I use setValue() or direct dot-notation assignment like gr.short_description = 'x'?

Both work and end up calling the same underlying setter, but `setValue('field', value)` is the safer habit because it avoids accidentally overwriting the whole `GlideElement` object instead of just its value, which can happen with careless dot-notation on some field types. Stick to `setValue()` consistently so your write behavior is predictable across field types like reference and choice fields.

### What happens to Business Rules when I call insert(), update(), or deleteRecord()?

By default, all matching before/after/async Business Rules on the table fire normally, just as if a user had saved the form. If you need to skip that automation for a specific script-driven write, call `setWorkflow(false)` before the operation, which suppresses Business Rules (and workflows) for that write only.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=gAO5l6MQC2E)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/channel/UCMf7pje1x5iZkkEF-Y3PLSA)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← GlideRecord Practical Demo — Part 1](01-gliderecord-1.md) · [GlideRecord Practical Demo — Part 3 →](03-gliderecord-3.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
