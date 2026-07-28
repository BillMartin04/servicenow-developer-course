---
description: "Decouple work with events and queues for scalable, resilient automation."
---

# Queueing & Event-Driven Architecture

**Quick answer:** Decouple work with events and queues for scalable, resilient automation. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=AtuGJLzP9r4" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/AtuGJLzP9r4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to decouple work with events and async processing.

## Overview

Decouple work with events and queues for scalable, resilient automation.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=AtuGJLzP9r4

## Key concepts

- **Event Registry and gs.eventQueue** — `gs.eventQueue('incident.priority.changed', current, current.priority, previousPriority)` registers an entry in the Event queue table immediately, while the Event Registry [sys_event_register] documents the event name, its expected parameters, and which listeners consume it.
- **Script Actions** — a Script Action subscribes to a specific event name and runs asynchronously once the event is processed, receiving the same `current` record and `event.parm1` / `event.parm2` values that were passed to `gs.eventQueue()`.
- **Async processing patterns** — firing an event returns control to the caller immediately, decoupling the triggering transaction (e.g. a Business Rule on update) from the slower or non-essential work (sending a notification, calling an integration) that happens later on the event queue.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Fire a custom event when an incident's priority changes and handle it asynchronously with a Script Action.

- [ ] In System Policy > Events > Registry, register a new event named `incident.priority.escalated` on the Incident table
- [ ] In a Business Rule (after, update) on Incident, check if `current.priority.changesTo('1')` and call `gs.eventQueue('incident.priority.escalated', current, current.priority, current.getValue('number'));`
- [ ] Create a Script Action subscribed to `incident.priority.escalated` that logs `event.parm1` and `event.parm2` with `gs.info()`
- [ ] Set an incident's priority to 1 (Critical) and save
- [ ] Check System Logs to confirm the Script Action ran and logged the expected values

**Done when:** the log entry from the Script Action appears after saving the record, showing the event fired and was handled without blocking the incident update itself.

## Frequently asked questions

### What's the difference between firing an event and just calling a function directly?

Calling a function directly runs synchronously as part of the current transaction, so the caller waits for it to finish and any error in it can affect the caller. `gs.eventQueue()` just queues an entry and returns immediately, so the listener (Script Action) runs later and independently, which is why it's used for non-critical or slower follow-up work.

### Why would my Script Action not seem to run right after I fire the event?

Events are processed by a scheduled job that polls the Event queue table, so there's normally a short delay (often just seconds) between `gs.eventQueue()` firing and the Script Action executing. If nothing runs at all, check that the event name in the Script Action's subscription exactly matches the name passed to `gs.eventQueue()`.

### When should I use gs.eventQueue instead of just putting the logic in the same Business Rule?

Use an event when the follow-up work is not essential to completing the triggering transaction — for example, sending a notification or calling an external system — so a failure or delay in that work doesn't block or slow down the save. Keep logic in the same Business Rule when it must complete before the transaction is considered done.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=AtuGJLzP9r4)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/@techtalkwithbill)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← Script Include & GlideAjax for Architects](03-script-include-glideajax-arch.md) · [Service Bus Architecture for Reusable Integrations →](05-service-bus.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
