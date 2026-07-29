---
description: "Catalog UI Policies control variable behaviour on catalog items — with optional scripting for advanced logic."
---

# Scripting in Catalog UI Policies

**Quick answer:** Catalog UI Policies control variable behaviour on catalog items — with optional scripting for advanced logic. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=u8pg9mXkhug" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/u8pg9mXkhug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to control catalog variables with scripted UI Policies.

## Overview

Catalog UI Policies control variable behaviour on catalog items — with optional scripting for advanced logic.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=u8pg9mXkhug

## Key concepts

- **Catalog UI Policy structure** — like a standard UI Policy, it has a condition and a set of variable actions (mandatory, visible, read-only), but it targets catalog **variables** and is attached to a catalog item or variable set instead of a table.
- **Scripting the execute-if-true logic** — the "Execute if true"/"Execute if false" script fields let you write JavaScript against `g_form` for logic the condition builder can't express, such as comparing two variables or looping over a variable set.
- **Applies-on settings** — checkboxes control where the policy runs (catalog item view, Service Portal / Employee Center, and record producer), so a policy built only for the classic catalog view won't fire on the portal unless that box is also checked.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Create a scripted Catalog UI Policy on a catalog item.

- [ ] Open a catalog item with at least two variables (or reuse the one from the previous lesson)
- [ ] Create a Catalog UI Policy with a condition on one variable (e.g. "Request Type is Other")
- [ ] In **Execute if true**, script `g_form.setMandatory('justification', true);` instead of using the actions table
- [ ] In **Execute if false**, script `g_form.setMandatory('justification', false);`
- [ ] Check **Catalog Item View** and **Service Portal** under applies-on, then test in both interfaces

**Done when:** Justification becomes mandatory only when Request Type is "Other", in both the classic catalog view and the Service Portal.

## Frequently asked questions

### When should I script a Catalog UI Policy instead of using the actions table?

Use the actions table for simple mandatory/visible/read-only changes on one or two variables, since it needs no code. Script it when you need conditional logic beyond a single condition, such as setting a value, comparing two variables, or applying different behaviour to several variables in a variable set at once.

### My Catalog UI Policy works in the classic catalog but not on the Service Portal — why?

Check the **applies-on** checkboxes on the UI Policy record. Each rendering context (catalog item view, Service Portal, record producer) must be explicitly enabled, so a policy built and tested only in the classic view will silently do nothing on the portal until you tick that box too.

### Do I need to write both Execute if true and Execute if false scripts?

You should write both if you want the variable state to revert automatically when the condition stops being true, since scripted Catalog UI Policies don't have a "Reverse if false" checkbox like standard UI Policies do. Leaving Execute if false empty means the variable stays in whatever state your true-branch script last set it to.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=u8pg9mXkhug)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/channel/UCMf7pje1x5iZkkEF-Y3PLSA)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← Catalog Client Scripts Explained](02-catalog-client-scripts.md) · [Understanding GlideSystem for Efficient Scripting →](../module-3-server-side/00-glidesystem.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
