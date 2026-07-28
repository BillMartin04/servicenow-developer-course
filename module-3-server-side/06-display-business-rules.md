---
description: "Display business rules run before a form loads and pass server data to the client via g_scratchpad."
---

# Display Business Rules & g_scratchpad

**Quick answer:** Display business rules run before a form loads and pass server data to the client via g_scratchpad. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=W4it_xeKGkI" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/W4it_xeKGkI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to pass server data to the client with g_scratchpad.

## Overview

Display business rules run before a form loads and pass server data to the client via g_scratchpad.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=W4it_xeKGkI

## Key concepts

- **Display rule timing** — a Display Business Rule runs server-side while the form is loading, before the page renders on the client, which makes it the right place to fetch data the client script will need on `onLoad`.
- **g_scratchpad pattern** — inside a Display rule you attach data with `g_scratchpad.myValue = current.getValue('priority')`, and that same `g_scratchpad` object becomes available as a plain client-side object once the form finishes loading.
- **Passing server data to client scripts safely** — only put non-sensitive data on `g_scratchpad`, since it's readable in the page's rendered HTML/JS by anyone with browser access to the form, unlike a server-only ACL-checked field.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Push a server-calculated value to the client with g_scratchpad and read it on form load.

- [ ] Create a **Display** Business Rule on `incident` (before, Display checkbox on)
- [ ] In the script, set `g_scratchpad.callerVip = current.caller_id.vip` (or another server-known value)
- [ ] Create a client script (**onLoad**) on the same table
- [ ] In the client script, read `g_scratchpad.callerVip` and show it with `g_form.addInfoMessage()`
- [ ] Open an incident record and confirm the message appears immediately on load with no extra round-trip

**Done when:** opening the incident form shows the info message populated from `g_scratchpad` with no visible delay, confirming the Display rule ran and populated the value before the client `onLoad` script executed.

## Frequently asked questions

### Why use a Display Business Rule instead of just calling GlideAjax from the client script?

A Display rule runs during the initial form load and delivers its data with zero extra round-trips, since it's baked into the same page load that renders the form. `GlideAjax` requires a separate asynchronous call after the page has already loaded, which is more flexible for on-demand lookups but introduces a visible delay or extra complexity if you just need a value the moment the form opens.

### Can I put any server data on g_scratchpad?

Technically yes, but you shouldn't put anything sensitive there, because `g_scratchpad` values are rendered into the client-side page source and are visible to anyone who can view that form, regardless of field-level ACLs. Only pass values the current user is already allowed to see.

### Why doesn't my onLoad client script see the g_scratchpad value I set?

Check that the Business Rule actually has its **Display** checkbox enabled, not just **before**, since only Display rules populate `g_scratchpad` for the client. Also confirm the property name matches exactly between the Display rule and the client script, since a typo silently returns `undefined` rather than an error.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=W4it_xeKGkI)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/@techtalkwithbill)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← How Business Rules Execute (with Examples)](05-business-rules-execution.md) · [Script Includes Explained (Full Demo) →](../module-4-script-includes-glideajax/00-script-includes.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
