---
description: "UI Policies declaratively control visibility, mandatory, and read-only field states — often a better choice than client scripts."
---

# UI Policies Explained

**Quick answer:** UI Policies declaratively control visibility, mandatory, and read-only field states — often a better choice than client scripts. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=VuzG7wjf_50" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/VuzG7wjf_50" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to declaratively control field behaviour with UI Policies.

## Overview

UI Policies declaratively control visibility, mandatory, and read-only field states — often a better choice than client scripts.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=VuzG7wjf_50

## Key concepts

- **UI Policy vs client script** — a UI Policy is a declarative, form-based way to change field visibility, mandatory, or read-only state, while a client script requires JavaScript for the same or more complex behaviour; UI Policies are preferred when no custom logic is needed.
- **Conditions and actions** — the condition builder decides when the policy applies (e.g. "Category is Hardware"), and the actions tab sets the resulting field states (visible, mandatory, read-only) for one or more fields.
- **Reverse-if-false behaviour** — when checked, ServiceNow automatically reverts the field states back to their original values the moment the condition stops being true, so you don't need a second policy to undo the change.
- **When to script a UI Policy** — use the optional "Execute if true"/"Execute if false" script fields when the built-in condition/action model can't express what you need, such as looping over multiple related fields or comparing two field values.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Build a UI Policy on the Incident form that reacts to Category.

- [ ] Create a UI Policy on **Incident** with the condition "Category is Hardware"
- [ ] Add an action that makes **Configuration item** mandatory when the condition is true
- [ ] Enable **Reverse if false** and confirm the field becomes optional again when you change Category away from Hardware
- [ ] Add a second UI Policy that hides the **Subcategory** field when Category is empty

**Done when:** selecting Hardware makes Configuration item mandatory, switching to another category reverts it automatically, and clearing Category hides Subcategory.

## Frequently asked questions

### Why isn't my UI Policy action reverting when the condition becomes false?

Check that **Reverse if false** is enabled on the UI Policy record. Without it, ServiceNow only applies your actions when the condition is true and leaves the field in whatever state it was in otherwise, which looks like the policy "getting stuck."

### Should I use a UI Policy or a Client Script for a mandatory field?

Use a UI Policy whenever the requirement is a straightforward condition-to-state mapping, since it needs no code and is easier for other admins to read. Only drop down to a Client Script if the mandatory logic depends on something a UI Policy condition can't express, like a calculation across multiple fields.

### Do UI Policies run on the client, the server, or both?

UI Policies run primarily on the client (in the browser) so the form updates instantly without a round trip, but ServiceNow also enforces the same mandatory/read-only state server-side on save as a safety net. This is why a hidden mandatory field can still block a save if it wasn't populated.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=VuzG7wjf_50)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/@techtalkwithbill)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← Mastering Client Scripts for Beginners](00-client-scripts.md) · [Catalog Client Scripts Explained →](02-catalog-client-scripts.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
