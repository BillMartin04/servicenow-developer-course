---
description: "Catalog client scripts customise Service Catalog item behaviour and differ from standard client scripts in important ways."
---

# Catalog Client Scripts Explained

**Quick answer:** Catalog client scripts customise Service Catalog item behaviour and differ from standard client scripts in important ways. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=Q-EsiiUp0zI" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/Q-EsiiUp0zI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to customise catalog item behaviour with catalog client scripts.

## Overview

Catalog client scripts customise Service Catalog item behaviour and differ from standard client scripts in important ways.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=Q-EsiiUp0zI

## Key concepts

- **Catalog client script scope** — a catalog client script is attached to a specific catalog item (or a variable set), so it only runs on that item's form, not on other catalog items or standard record forms.
- **Variables vs fields** — catalog items are built from **variables** (defined on the catalog item), not table columns, so you reference them with `g_form.getValue('variable_name')` using the variable's name, not a database field name.
- **g_form in the catalog context** — the same `g_form` API from standard Client Scripts is available on catalog item forms, but it operates on variables and works both on the classic Service Catalog and the Service Portal / Employee Center widgets.
- **Real-world use cases** — typical examples are showing or hiding a variable based on another variable's answer, populating a reference variable's default value, or validating input before allowing a submission.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Build a catalog client script on a Service Catalog item.

- [ ] Open (or create) a catalog item with at least two variables, e.g. "Request Type" (choice) and "Justification" (multi-line text)
- [ ] Add a catalog client script of type `onChange` on "Request Type" that shows "Justification" only when the value is "Other"
- [ ] Test it by opening the catalog item form and switching Request Type between values
- [ ] Confirm the script does NOT fire on a different, unrelated catalog item

**Done when:** Justification appears only when Request Type is "Other", and toggling back hides it again.

## Frequently asked questions

### Why doesn't my regular Client Script work on a catalog item form?

Standard Client Scripts are scoped to a table (like Incident) and don't run on Service Catalog item forms, which are built from variables rather than table fields. You need a **Catalog Client Script**, attached directly to the catalog item or a variable set, to control variable behaviour.

### Can I reference a variable using its table field name?

No. Variables aren't table columns, so you must use the exact **variable name** you defined on the catalog item (for example `g_form.getValue('justification')`), not a label or a guessed database field name. Check the variable's name on the catalog item's Variables tab if you're unsure.

### Does a catalog client script work the same way on the Service Portal as in the classic UI?

Mostly yes for `g_form` calls, since Service Portal implements the same client API, but some visual behaviours (like layout reflow) can look different because the portal widget renders variables differently than the classic catalog form. Always test in whichever interface your end users actually use.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=Q-EsiiUp0zI)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/channel/UCMf7pje1x5iZkkEF-Y3PLSA)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← UI Policies Explained](01-ui-policies.md) · [Scripting in Catalog UI Policies →](03-catalog-ui-policies.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
