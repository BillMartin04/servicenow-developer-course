---
description: "Part 2 covers authentication, error handling, and wrapping the call in a reusable service."
---

# REST API Integration (Part 2)

**Quick answer:** Part 2 covers authentication, error handling, and wrapping the call in a reusable service. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=nU9XaRzyWeA" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/nU9XaRzyWeA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to add auth, error handling, and a reusable service.

## Overview

Part 2 covers authentication, error handling, and wrapping the call in a reusable service.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=nU9XaRzyWeA

## Key concepts

- **Basic/OAuth auth** — rather than hard-coding a username, password, or token in script, attach a connection & credential alias (or an OAuth profile) to the REST message so ServiceNow injects the credentials at execute time and keeps them out of source code.
- **Query params and status/error handling** — build the query string with `setQueryParameter('name', 'value')` calls instead of concatenating strings into the URL, then after `execute()` always check `getStatusCode()` and branch on non-200 codes rather than assuming the call succeeded.
- **Timeouts and async execute** — `setHttpTimeout(ms)` takes a value in **milliseconds** (e.g. `setHttpTimeout(10000)` for 10 seconds), and `executeAsync()` sends the request without blocking the current transaction, with the response handled later via a callback/event instead of the return value of `execute()`.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Wrap a REST call in a reusable Script Include with proper auth, timeout, and error handling.

- [ ] Create a Script Include that builds a `RESTMessageV2` using a connection & credential alias (or a system property for a test token) instead of a literal password
- [ ] Add one or more query parameters with `setQueryParameter()`
- [ ] Call `setHttpTimeout(10000)` and then `execute()`
- [ ] Check `getStatusCode()` and return a consistent result object like `{ success: true, data: ... }` or `{ success: false, error: ... }`
- [ ] Force a failure (bad endpoint or short timeout) and confirm your function returns the error shape instead of throwing

**Done when:** calling the function with valid input returns `success: true` with parsed data, and calling it with a broken endpoint or a 1ms timeout returns `success: false` with a useful error message instead of an uncaught exception.

## Frequently asked questions

### Why shouldn't I put my API password directly in the script?

Hard-coded credentials get exposed to anyone who can read the script, get captured in update sets and version history, and are painful to rotate. Use a connection & credential alias (or OAuth profile) on the REST message, or at minimum store the secret in a protected system property, so the credential is managed and swappable outside of code.

### What's the difference between execute() and executeAsync()?

`execute()` blocks the current transaction until the external system responds, which is simplest but ties up a semaphore for the call's duration. `executeAsync()` fires the request and lets the transaction continue, with the response processed later — useful for slow endpoints where you don't want to block a user-facing transaction.

### My integration hangs or times out inconsistently — what should I check?

First confirm `setHttpTimeout()` is set to a sensible value in milliseconds, since the default can be longer than you expect and leave transactions hanging. Then confirm you're checking `getStatusCode()` and `getErrorMessage()` on every call, since a slow or failing endpoint should surface as a handled error rather than an unexplained hang.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=nU9XaRzyWeA)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/channel/UCMf7pje1x5iZkkEF-Y3PLSA)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← REST API Integration — Part 1](00-rest-api-1.md) · [Secure Coding with Script Includes & GlideAjax →](02-secure-coding.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
