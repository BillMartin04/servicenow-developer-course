---
description: "A centralised error-handling strategy for consistent logging and graceful failures."
---

# How to Implement Global Error Handling

**Quick answer:** A centralised error-handling strategy for consistent logging and graceful failures. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=0Ctaw6vnP1I" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/0Ctaw6vnP1I" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to centralise logging and graceful failure handling.

## Overview

A centralised error-handling strategy for consistent logging and graceful failures.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=0Ctaw6vnP1I

## Key concepts

- **Try/catch patterns** — wrap risky operations (REST calls, JSON.parse, GlideRecord updates that can fail) in `try/catch` at the point where they happen, then hand the caught error to a central handler instead of letting it bubble up as an unhandled exception.
- **Central error logger Script Include** — a single Script Include (for example `GlobalErrorHandler`) provides one method every other script calls on failure, so every log entry has the same structure: source, message, and context, instead of ad-hoc `gs.error()` calls scattered across the codebase.
- **Surfacing errors to users and admins** — the handler should log full technical detail (stack trace, input values, source script) to the system log for admins via `gs.error()`, while returning a short, safe, non-technical message to the end user or calling script — never expose internal error detail directly to the client.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Build a shared error-handling Script Include and use it from another script.

- [ ] Create a `GlobalErrorHandler` Script Include with a method like `logError(source, error, context)`
- [ ] Inside it, call `gs.error()` with a consistent format that includes the source name and context
- [ ] Have the method return a plain result object such as `{ success: false, message: 'A friendly error message' }` rather than throwing
- [ ] In a second Script Include, wrap a risky call (e.g. `JSON.parse` on bad input) in `try/catch` and call `GlobalErrorHandler` from the `catch` block
- [ ] Trigger the failure path and confirm the system log shows the detailed error while the caller only receives the friendly message

**Done when:** the system log (`gs.error()` output) shows the source and context for the failure, and the calling script receives a clean `{ success: false, message: ... }` object instead of an uncaught exception.

## Frequently asked questions

### Why centralize error handling instead of just using try/catch everywhere?

Scattering `try/catch` blocks with inconsistent logging makes it hard to search logs or spot patterns across the instance. A central error-handler Script Include gives every failure the same shape and destination, so admins can search one log source and get consistent context (source script, message, related record) every time.

### Should the error handler ever throw the original exception back up?

Generally no — a gateway-style handler should return a predictable result object (like `{ success: false, message: ... }`) so calling code can check a flag instead of needing its own `try/catch` for every call. Re-throwing raw errors defeats the purpose of centralizing handling and risks leaking technical detail to the client.

### What's the difference between what I log and what I show the user?

The system log via `gs.error()` should capture full technical detail — stack trace, input values, table/record context — because that's what admins need to debug. The message returned to the user or UI should be short and non-technical, since exposing raw error text or stack traces to end users is both confusing and a potential information leak.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=0Ctaw6vnP1I)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/@techtalkwithbill)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← Call a Script Include in a Flow Designer Custom Action](03-flow-designer-custom-action.md) · [Final Exercise: Build a Production-Ready App →](../module-7-capstone/00-final-exercise.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
