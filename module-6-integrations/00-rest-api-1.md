---
description: "Call external REST APIs from ServiceNow using RESTMessageV2 wrapped in a Script Include."
---

# Script Include & GlideAjax — REST API Integration (Part 1)

**Quick answer:** Call external REST APIs from ServiceNow using RESTMessageV2 wrapped in a Script Include. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=hfnyomgSFWo" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/hfnyomgSFWo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to call external REST APIs with RESTMessageV2.

## Overview

Call external REST APIs from ServiceNow using RESTMessageV2 wrapped in a Script Include.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=hfnyomgSFWo

## Key concepts

- **RESTMessageV2 basics** — in a scoped application, create the message with `new sn_ws.RESTMessageV2()` (either a fresh instance or one built from a saved REST Message record and method name), then call `execute()` to send the request and get a response object back.
- **Endpoints, methods, headers** — set the target with `setEndpoint('https://...')`, choose the verb with `setHttpMethod('GET')` (or POST/PUT/DELETE), and attach headers such as content type or auth tokens with `setRequestHeader('name', 'value')` before calling `execute()`.
- **Parsing JSON responses** — call `response.getBody()` to get the raw string, then wrap `JSON.parse(body)` in a `try/catch` so a malformed or non-JSON body (like an HTML error page) doesn't throw an uncaught exception in your Script Include.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Build a Script Include that calls a public REST API and logs the parsed result.

- [ ] Create a scoped Script Include with a function that builds `new sn_ws.RESTMessageV2()`
- [ ] Set the endpoint to a public test API (e.g. `https://jsonplaceholder.typicode.com/todos/1`) and the method to `GET`
- [ ] Call `execute()` and store the response object
- [ ] Call `getStatusCode()` and only parse the body when it is `200`
- [ ] Wrap `JSON.parse(response.getBody())` in a `try/catch` and log the resulting object with `gs.info()`

**Done when:** running the Script Include from Scripts - Background logs the parsed JSON fields (not a raw string), and changing the endpoint to an invalid URL logs a handled error instead of throwing an exception.

## Frequently asked questions

### Why use RESTMessageV2 instead of an outbound REST message record?

A saved REST Message record is reusable and shows up in the integration's configuration UI, which is easier to maintain and reuse across scripts. Building `new sn_ws.RESTMessageV2()` directly in script gives you full control for one-off or highly dynamic calls, but any hard-coded endpoint or header should really live in a saved message or system property instead.

### Do I need setHttpMethod if I built the message from a saved REST Message record?

If you constructed the message with `new sn_ws.RESTMessageV2('MyRESTMessage', 'get')`, the method is already set from the record and you don't need to call `setHttpMethod()` again. You only need it when you build the message from scratch with the no-argument constructor.

### Why does JSON.parse sometimes fail on a response that looks fine?

Some APIs return an empty body, plain text, or an HTML error page instead of JSON, especially on failures or redirects. Always check `getStatusCode()` before parsing, and wrap `JSON.parse()` in a `try/catch` so an unexpected body doesn't crash the calling script.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=hfnyomgSFWo)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/channel/UCMf7pje1x5iZkkEF-Y3PLSA)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← Service Bus Architecture for Reusable Integrations](../module-5-architecture/05-service-bus.md) · [REST API Integration — Part 2 →](01-rest-api-2.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
