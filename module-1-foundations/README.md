# Module 1 · JavaScript & Scripting Foundations

**Estimated time:** ~40 min  ·  **Topics:** 2

Build the JavaScript foundation every ServiceNow script depends on, and internalise the single most important mental model: where code runs.

## Learning objectives

By the end of this module, you will be able to:

- Write correct JavaScript using the subset ServiceNow supports
- Explain truthiness, equality, and common scripting gotchas
- Distinguish client-side from server-side execution and choose the right one
- Trace a form's request lifecycle from server to browser and back

## Prerequisites

Module 0 complete with a working PDI. Basic familiarity with any programming concept is helpful but not required.

## Topics

| # | Topic | Type | Time | You'll learn to |
| --- | --- | --- | --- | --- |
| 1.1 | [JavaScript Essentials for ServiceNow](00-javascript-essentials.md) | 📖 Guide | 20 min | Master the JavaScript subset used across the platform |
| 1.2 | [Where Scripts Run: Client vs Server](01-where-scripts-run.md) | 📖 Guide | 20 min | Choose client vs server correctly and trace the form lifecycle |

## Knowledge check

Before moving on, make sure you can answer these:

- Which values are falsy in JavaScript, and why does `'0'` behave unexpectedly?
- Name two APIs available client-side and two available server-side.
- A client script needs data from the database. What is the correct pattern?

## Module recap

{% hint style="success" %}
You can write platform-safe JavaScript and confidently decide whether any given piece of logic belongs in the browser or on the server.
{% endhint %}

## Why this makes you AI-ready

{% hint style="info" %}
Ask an AI for ServiceNow logic and it will happily place a database call inside a client script, or hand you modern JavaScript the platform's server-side engine won't run. Both look fine and both break in production. This module gives you the one mental model — *where does this code run?* — that lets you catch it instantly. Without it, you cannot tell whether generated code is even in the right place.
{% endhint %}

## Need help?

Ask your question in the comments of the video for the topic you are stuck on — see [Get Help & Get Verified](../support-and-verification.md).

**Next up →** [Module 2 · Client-Side Scripting](../module-2-client-side/README.md)
