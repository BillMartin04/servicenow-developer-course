---
description: "Client scripts control form behaviour in the browser."
---

# Mastering Client Scripts for Beginners

**Quick answer:** Client scripts control form behaviour in the browser. Learn onLoad, onChange, onSubmit, and onCellEdit — and how to keep them fast. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=WLNQinTkLfQ" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/WLNQinTkLfQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to build the four client script types and keep them fast.

## Overview

Client scripts control form behaviour in the browser. Learn onLoad, onChange, onSubmit, and onCellEdit — and how to keep them fast.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=WLNQinTkLfQ

## Key concepts

- **The four client script types** — `onLoad` runs when the form finishes loading, `onChange` runs when a specific field's value changes, `onSubmit` runs just before the form is submitted (and can cancel it), and `onCellEdit` runs when a value changes in a list's inline editor.
- **g_form and g_user APIs** — `g_form` reads and writes form state in the browser, such as `g_form.setValue()`, `g_form.getValue()`, and `g_form.setMandatory()`; `g_user` exposes read-only info about the logged-in user, like `g_user.hasRole('itil')` and `g_user.userID`.
- **onChange field arguments** — an `onChange` function receives `(control, oldValue, newValue, isLoading, isTemplate)`, and you should return early when `isLoading` is true or `newValue` is empty to avoid firing logic unnecessarily.
- **Performance best practices** — avoid `GlideRecord` queries inside client scripts, keep onChange logic lightweight since it fires on every keystroke-triggered change, and prefer GlideAjax for anything that needs server data.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Build a small set of client scripts on the Incident form to control field behaviour without touching the server.

- [ ] Create an `onChange` Client Script on **Category** that calls `g_form.setValue('impact', '1')` when Category is "Network"
- [ ] Create an `onLoad` Client Script that sets **Urgency** to a default value only when the form is new (`g_form.isNewRecord()`)
- [ ] Add an `onSubmit` Client Script that blocks submission with `g_form.addErrorMessage()` if Short Description is under 10 characters
- [ ] Confirm the `isLoading` guard works by reloading the form and checking your onChange logic doesn't fire on load

**Done when:** changing Category to "Network" auto-sets Impact, a new incident gets a default Urgency, and submitting with a short description shows your custom error.

## Frequently asked questions

### Why does my onChange script fire when the form first loads, not just when the user changes the field?

This happens when you don't check the `isLoading` parameter. ServiceNow calls `onChange` once during form load to sync initial state, so always add `if (isLoading || newValue === '') { return; }` at the top of the function if you only want the logic to run on genuine user edits.

### When should I use a Client Script instead of a UI Policy?

Use a UI Policy for simple, declarative field state changes like show/hide, mandatory, or read-only based on conditions — it's faster to build and easier to maintain. Reach for a Client Script when you need custom logic, such as calling `g_form.addErrorMessage()`, manipulating multiple fields conditionally, or calling GlideAjax to the server.

### Can I query the database directly from a Client Script?

You can use `GlideRecord` in a Client Script, but it runs as a synchronous, blocking browser call and is strongly discouraged for performance reasons. Use a **GlideAjax** call to a Script Include instead, which runs the query on the server and returns just the data you need.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=WLNQinTkLfQ)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/@techtalkwithbill)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← Where Scripts Run: Client vs Server](../module-1-foundations/01-where-scripts-run.md) · [UI Policies Explained →](01-ui-policies.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
