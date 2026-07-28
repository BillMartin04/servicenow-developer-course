---
description: "A service-bus style pattern for scalable, reusable integrations across the platform."
---

# Service Bus Architecture for Reusable Integrations

**Quick answer:** A service-bus style pattern for scalable, reusable integrations across the platform. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=qAIB5YR7EYk" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/qAIB5YR7EYk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to design a scalable, reusable integration layer.

## Overview

A service-bus style pattern for scalable, reusable integrations across the platform.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=qAIB5YR7EYk

## Key concepts

- **Service bus concept** — a single facade Script Include sits between calling code and every outbound integration, so consumers call one consistent API (e.g. `IntegrationBus.send('slack', payload)`) instead of knowing the details of each external system.
- **Routing and adapters** — the facade routes each call to a small adapter class dedicated to one integration (Slack, email, a REST endpoint via `sn_ws.RESTMessageV2()`), so adding a new integration means adding a new adapter, not modifying existing calling code.
- **Reusable integration layer** — because callers depend only on the facade's interface, integrations can be swapped, retried, or logged centrally in one place, and every consumer benefits automatically instead of duplicating that logic per integration.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Build a minimal service-bus facade routing to two fake outbound adapters.

- [ ] Create adapter Script Includes `SlackAdapter` and `EmailAdapter`, each with a `send(payload)` method that just logs the payload with `gs.info()` (standing in for a real outbound call)
- [ ] Create a facade Script Include `IntegrationBus` with `initialize()` that builds a lookup object mapping `'slack'` to `new SlackAdapter()` and `'email'` to `new EmailAdapter()`
- [ ] Add a method `send(channel, payload)` on `IntegrationBus` that looks up the right adapter by `channel` and calls its `send(payload)`
- [ ] From Scripts - Background, call `new IntegrationBus().send('slack', {text: 'hello'})` and `new IntegrationBus().send('email', {text: 'hello'})`
- [ ] Confirm both calls log through their respective adapters without the calling script knowing which adapter class was used

**Done when:** switching the `channel` argument routes to a different adapter automatically, and adding a third adapter would require no changes to the calling script.

## Frequently asked questions

### How is a service bus different from just calling each integration's Script Include directly?

Calling each integration directly means every consumer needs to know which class to instantiate and how to call it, so changes to one integration can ripple out to many callers. A service bus centralizes that knowledge behind one facade, so consumers only ever learn one API.

### Does "service bus" here mean ServiceNow's IntegrationHub or a message broker product?

No — in this lesson it refers to an architectural pattern you build yourself with plain Script Includes (a facade plus adapters), not a specific ServiceNow product or licensed capability. IntegrationHub can be one of the things an adapter calls into, but the pattern itself is just good class design.

### When is it overkill to build a service-bus facade for integrations?

If you only have one outbound integration and no plans to add more, a single well-structured Script Include is usually enough and a facade adds unnecessary indirection. The pattern pays off once you have multiple integrations that share common concerns like logging, retries, or routing logic.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=qAIB5YR7EYk)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/@techtalkwithbill)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← Queueing & Event-Driven Architecture](04-queueing-event-driven.md) · [REST API Integration — Part 1 →](../module-6-integrations/00-rest-api-1.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
