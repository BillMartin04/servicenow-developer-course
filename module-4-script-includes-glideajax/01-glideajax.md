---
description: "GlideAjax lets client scripts call server-side Script Includes asynchronously without a full form submit."
---

# Script Include & GlideAjax

**Quick answer:** GlideAjax lets client scripts call server-side Script Includes asynchronously without a full form submit. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=0eU1uwreZH4" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/0eU1uwreZH4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to call the server from the client asynchronously.

## Overview

GlideAjax lets client scripts call server-side Script Includes asynchronously without a full form submit.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=0eU1uwreZH4

## Key concepts

- **AbstractAjaxProcessor** — a client-callable Script Include must extend this base class via `Object.extendsObject(AbstractAjaxProcessor, ClassName)`; it supplies the `initialize()` and `getParameter()` plumbing, so your Script Include must never define its own `initialize()`.
- **getXMLAnswer and getXML** — `getXMLAnswer(callback)` is the async call that returns just the string your server method sends back via `return`; `getXML(callback)` returns the full XML response document and is rarely needed for simple value passing.
- **Passing parameters** — `ga.addParam('sysparm_name', 'methodName')` tells the server which function to run, and additional `addParam('sysparm_myArg', value)` calls pass extra values, all retrieved server-side with `this.getParameter('sysparm_myArg')`.
- **Async best practices** — always use the callback-based `getXMLAnswer()` instead of the deprecated synchronous `getXMLWait()`, since synchronous calls freeze the browser UI while waiting on the server.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Build an end-to-end GlideAjax call that validates a field value on the server.

- [ ] Create a client-callable Script Include `AjaxValidator` extending `AbstractAjaxProcessor` (no custom `initialize()`) with a method `isPriorityHigh()` that reads `sysparm_priority` and returns `JSON.stringify({ isHigh: value === '1' })`
- [ ] On the Incident form, add a Client Script that builds `new GlideAjax('AjaxValidator')`, calls `addParam('sysparm_name','isPriorityHigh')` and `addParam('sysparm_priority', g_form.getValue('priority'))`
- [ ] Call `getXMLAnswer(callback)` and in the callback `JSON.parse()` the response string
- [ ] Show an alert if `isHigh` is true

**Done when:** changing the Incident's priority to 1 - Critical triggers the alert, and other priority values do not.

## Frequently asked questions

### Why does my GlideAjax callback receive `undefined` or the whole XML instead of my value?

This almost always means the Script Include's `initialize()` was overridden, breaking the parent `AbstractAjaxProcessor` wiring, or the client callback is reading `response` directly instead of calling `getXMLAnswer()`'s parameter. Use `getXMLAnswer(function(response) { var answer = response; })` and never define `initialize()` on the Script Include.

### Can GlideAjax return an object or array directly?

No. GlideAjax only ever returns a string. To send structured data, `JSON.stringify()` the object server-side before `return`ing it, then `JSON.parse()` the string in the client callback to get it back as an object.

### Should I use getXMLAnswer or getXMLWait?

Always prefer `getXMLAnswer()`. It is asynchronous and does not block the browser while it waits for the server, whereas `getXMLWait()` is synchronous, deprecated, and can make the whole UI feel frozen.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=0eU1uwreZH4)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/@techtalkwithbill)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← Script Includes Explained (Full Demo)](00-script-includes.md) · [Script Include & Object.extendObject — Part 1 →](02-extendobject-1.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
