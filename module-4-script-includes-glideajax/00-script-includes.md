---
description: "Script Includes are reusable server-side classes."
---

# Script Includes Explained (Full Demo)

**Quick answer:** Script Includes are reusable server-side classes. Learn how to build, scope, and call them. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=yu75ZE_qFUY" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/yu75ZE_qFUY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to create reusable server-side classes.

## Overview

Script Includes are reusable server-side classes. Learn how to build, scope, and call them.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=yu75ZE_qFUY

## Key concepts

- **Script Include structure** — a Script Include is a record that wraps a JavaScript object, typically created with `var ClassName = Class.create();` followed by a `prototype` object holding `initialize()` and other methods, plus `type: 'ClassName'` at the end.
- **Client-callable vs on-demand** — checking **Client callable** exposes the Script Include to `GlideAjax` from client scripts; leaving it unchecked means it only runs server-side (business rules, other Script Includes) and is not reachable from the browser, which is safer and faster to load.
- **Scope and access control** — the API Name is the fully qualified reference (e.g. `x_app_name.ClassName`) you use to call the include from another scope, while **Accessible from** controls whether other application scopes may call it at all.
- **Reuse patterns** — putting shared logic (validation, lookups, calculations) in one Script Include and calling it from multiple business rules, client scripts, or other Script Includes avoids copy-pasting the same code everywhere.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Create a reusable Script Include and prove it works from two different callers.

- [ ] Create a new Script Include named `StringUtils` with `Class.create()` and a method `reverse(str)` that returns the reversed string
- [ ] Leave **Client callable** unchecked since this will only be called server-side
- [ ] Call `new StringUtils().reverse('hello')` from Scripts - Background and log the result with `gs.info()`
- [ ] Call the same method from inside a Business Rule on an unrelated table and log the result again

**Done when:** both callers log `olleh`, confirming the Script Include is genuinely reusable rather than tied to one script.

## Frequently asked questions

### Do I need to check "Client callable" on every Script Include?

No. Only check it if the Script Include will be invoked directly from a client script via `GlideAjax`. Server-only callers — business rules, other Script Includes, scheduled jobs — never need it, and leaving it unchecked reduces the client-side attack surface.

### What's the difference between the Script Include's name and its API name?

The **Name** field is just the record's label in the list; the **API Name** (shown as `scope.ClassName`) is the identifier you actually reference in code, especially when calling across scopes. In the global scope they often look identical, but in a scoped app the API name is prefixed with the scope, e.g. `x_acme_util.StringUtils`.

### Why use a Script Include instead of just writing the logic inline each time?

A Script Include lets one tested implementation be called from many business rules, client scripts, and other Script Includes, so a fix or improvement only needs to happen in one place. It also keeps large or reusable logic out of individual business rules, making both easier to read and maintain.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=yu75ZE_qFUY)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/channel/UCMf7pje1x5iZkkEF-Y3PLSA)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← Display Business Rules & g_scratchpad](../module-3-server-side/06-display-business-rules.md) · [Script Include & GlideAjax →](01-glideajax.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
