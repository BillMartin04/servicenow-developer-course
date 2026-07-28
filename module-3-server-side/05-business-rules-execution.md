---
description: "Business rules are the backbone of server-side automation."
---

# How Business Rules Execute (with Examples)

**Quick answer:** Business rules are the backbone of server-side automation. Learn when they run: before, after, async, and display. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=qjHB5mZrKIk" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/qjHB5mZrKIk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to master the four Business Rule timings and order of execution.

## Overview

Business rules are the backbone of server-side automation. Learn when they run: before, after, async, and display.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=qjHB5mZrKIk

## Key concepts

- **The four business rule timings** — **before** runs before the database write and can change `current` values before they save, **after** runs once the record is committed, **async** runs after in a separate scheduled job so it doesn't block the user's save, and **display** runs when the form loads to prepare client-side data.
- **current and previous objects** — `current` is the `GlideRecord` of the record being saved with its new values, while `previous` (available in after/async rules on update) holds the record's values before this save, letting you compare old vs. new.
- **Order of execution** — within the same timing (e.g. multiple before rules), rules run in ascending **Order** field value, so a lower order number runs first; get the sequencing wrong and a later rule may depend on a field a later-ordered rule hasn't set yet.
- **Common pitfalls** — calling `current.update()` inside a before rule triggers a redundant, recursive save since the pending write already happens automatically; only call `.update()` explicitly in after/async rules or when working with a different record.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Build before and after Business Rules on Incident and observe their order.

- [ ] Create a **before update** Business Rule on `incident` that sets a custom field (or `work_notes`) using `current.setValue()`
- [ ] Create an **after update** Business Rule on the same table and condition that calls `gs.info()` comparing `previous.priority` to `current.priority`
- [ ] Update an incident's priority from the form and check **System Logs > System Log** for the after rule's message
- [ ] Change the before rule's **Order** field to a higher number than a second before rule you add, and confirm in the log which one's effect "wins"

**Done when:** the after rule's log line correctly shows both the old and new priority values, and you can explain why your before rule's field change was visible by the time the after rule ran.

## Frequently asked questions

### When should I use an async Business Rule instead of after?

Use async when the logic is slow or not time-critical for the user, like sending an email or calling an external integration, since async rules run in a scheduled job after the transaction completes instead of blocking the save. Use after when the logic must complete, and be visible to the user, immediately after the record commits, such as updating a related record other logic depends on right away.

### Why is previous undefined in my before Business Rule?

`previous` is only populated in after and async Business Rules on an update, because it represents the database's prior state which before rules run ahead of. In a before rule, compare against the database directly with a fresh `GlideRecord` query if you need the pre-save value, or check `current.field.changes()` to see if a field was modified.

### Does the Order field control execution across before and after rules together?

No, order only sequences rules within the same timing and same table/condition; before rules all run (in their own order) before any after rules run, and async rules run afterward in the background. If you need rule B to definitely run after rule A but they're different timings, that ordering is guaranteed by the timing itself, not the Order field.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=qjHB5mZrKIk)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/@techtalkwithbill)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← GlideRecord Practical Demo — Part 4](04-gliderecord-4.md) · [Display Business Rules & g_scratchpad →](06-display-business-rules.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
