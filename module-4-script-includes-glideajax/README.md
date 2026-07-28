# Module 4 · Script Includes & GlideAjax

**Estimated time:** ~45 min  ·  **Topics:** 4

Write reusable server-side code with Script Includes and call it from the client asynchronously with GlideAjax — the bridge between browser and server.

## Learning objectives

By the end of this module, you will be able to:

- Build reusable, correctly-scoped Script Includes
- Expose client-callable Script Includes and call them via GlideAjax
- Implement inheritance with Object.extendsObject
- Choose between inheritance and composition for reuse

## Prerequisites

Modules 0–3. You must be comfortable with server-side scripting and GlideRecord.

## Topics

| # | Topic | Type | Time | You'll learn to |
| --- | --- | --- | --- | --- |
| 4.1 | [Script Includes Explained (Full Demo)](00-script-includes.md) | 🎬 Video | 13 min | Create reusable server-side classes |
| 4.2 | [Script Include & GlideAjax](01-glideajax.md) | 🎬 Video | 12 min | Call the server from the client asynchronously |
| 4.3 | [Script Include & Object.extendObject — Part 1](02-extendobject-1.md) | 🎬 Video | 10 min | Implement class inheritance in ServiceNow |
| 4.4 | [Script Include & Object.extendObject — Part 2](03-extendobject-2.md) | 🎬 Video | 10 min | Override methods and call the parent prototype |

## Knowledge check

Before moving on, make sure you can answer these:

- What makes a Script Include client-callable?
- Which GlideAjax method returns a simple string answer asynchronously?
- When should you prefer composition over inheritance?

## Module recap

{% hint style="success" %}
You can package logic into reusable Script Includes and safely invoke them from the client — the foundation for clean architecture.
{% endhint %}

## Why this makes you AI-ready

{% hint style="info" %}
Client-callable Script Includes are one of the most common things AI gets subtly wrong: it adds a custom `initialize()` that breaks `AbstractAjaxProcessor`, forgets that GlideAjax returns strings, or uses the synchronous `getXMLWait()` that freezes the browser. Every one of these runs in a quick test and fails under real load. Building the bridge correctly yourself is what lets you catch these the moment you read the generated code.
{% endhint %}

## Need help?

Ask your question in the comments of the video for the topic you are stuck on — see [Get Help & Get Verified](../support-and-verification.md).

**Next up →** [Module 5 · Architecture & Design Patterns](../module-5-architecture/README.md)
