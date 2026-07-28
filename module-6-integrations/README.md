# Module 6 · Integrations, Security & Reliability

**Estimated time:** ~1 hr  ·  **Topics:** 5

Integrate with external systems over REST, bridge low-code and pro-code with Flow Designer, and make your code secure and resilient.

## Learning objectives

By the end of this module, you will be able to:

- Call external REST APIs with RESTMessageV2 wrapped in a Script Include
- Handle authentication, errors, and timeouts on integrations
- Write secure server-side code and avoid common vulnerabilities
- Call Script Includes from Flow Designer and implement global error handling

## Prerequisites

Modules 0–5. Architecture patterns from Module 5 are used throughout.

## Topics

| # | Topic | Type | Time | You'll learn to |
| --- | --- | --- | --- | --- |
| 6.1 | [REST API Integration (Part 1)](00-rest-api-1.md) | 🎬 Video | 12 min | Call external REST APIs with RESTMessageV2 |
| 6.2 | [REST API Integration (Part 2)](01-rest-api-2.md) | 🎬 Video | 12 min | Add auth, error handling, and a reusable service |
| 6.3 | [Secure Coding with Script Include & GlideAjax](02-secure-coding.md) | 🎬 Video | 12 min | Validate input and enforce access server-side |
| 6.4 | [Call a Script Include in a Flow Designer Custom Action](03-flow-designer-custom-action.md) | 🎬 Video | 10 min | Bridge low-code and pro-code with custom actions |
| 6.5 | [How to Implement Global Error Handling](04-global-error-handling.md) | 🎬 Video | 11 min | Centralise logging and graceful failure handling |

## Knowledge check

Before moving on, make sure you can answer these:

- Where should an external REST call be isolated in your architecture?
- Why must validation be enforced server-side even if the client validates?
- What are the benefits of a single global error handler?

## Module recap

{% hint style="success" %}
You can integrate ServiceNow with the outside world securely and reliably, with consistent error handling.
{% endhint %}

## Why this makes you AI-ready

{% hint style="info" %}
Integration code is where "it worked in the demo" fails hardest. AI routinely writes a REST call with no status-code check, no timeout, and a `JSON.parse` with no try/catch — so one malformed response takes the whole flow down silently. It also trusts client-side validation it should never trust. This module gives you the security and reliability checklist to hold generated integrations to, so what you ship survives contact with the real world.
{% endhint %}

## Need help?

Ask your question in the comments of the video for the topic you are stuck on — see [Get Help & Get Verified](../support-and-verification.md).

**Next up →** [Module 7 · Final Exercise & Assessment](../module-7-capstone/README.md)
