---
description: "Write secure server-side code: input validation, access control, and avoiding injection."
---

# Script Include & GlideAjax — Secure Coding

**Quick answer:** Write secure server-side code: input validation, access control, and avoiding injection. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=Ccxb4UJKF9c" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/Ccxb4UJKF9c" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to validate input and enforce access server-side.

## Overview

Write secure server-side code: input validation, access control, and avoiding injection.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=Ccxb4UJKF9c

## Key concepts

- **ACLs and script protection** — access control rules run on the server regardless of what a client-callable script exposes, so even if a GlideAjax method is reachable from the browser, the underlying table/field ACLs are still the last line of defense.
- **Input validation** — never trust parameters coming from `getParameter()` in a client-callable Script Include; validate type, format, and expected values on the server before using them in a query or record operation, since client-side checks can be bypassed entirely.
- **Client-callable exposure risks** — every public function on an `AbstractAjaxProcessor` subclass is callable by any authenticated (or even unauthenticated, for some endpoints) user who can guess the Script Include and method name, so treat each one as a mini API endpoint that needs its own validation and authorization checks.
- **GlideRecordSecure** — using `new GlideRecordSecure('table')` instead of `GlideRecord` enforces the current user's ACLs on reads/writes, so a query only returns records and fields the user is actually allowed to see, instead of the elevated access a plain `GlideRecord` gets in server-side script.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Harden a GlideAjax method against bad input and unauthorized access.

- [ ] Create a client-callable Script Include extending `AbstractAjaxProcessor` (no custom `initialize()`)
- [ ] Add a public method that reads an input with `this.getParameter('sysparm_name')`
- [ ] Validate the input (correct type/format) and return a clear error string if it fails validation
- [ ] Swap the internal query to use `new GlideRecordSecure('incident')` instead of `GlideRecord`
- [ ] Test as a user with restricted ACLs and confirm they only get back records/fields they're allowed to see

**Done when:** calling the method with a bad or missing parameter returns a handled validation error (not a script exception), and a restricted test user cannot retrieve data they don't have ACL access to.

## Frequently asked questions

### If a Script Include isn't in a client-callable menu, is it safe from the browser?

No — any Script Include with **Client callable** checked can be invoked directly via GlideAjax from the browser console by name, regardless of whether a UI element links to it. Security has to come from validation and ACLs inside the method itself, not from hiding the entry point.

### When should I use GlideRecordSecure instead of GlideRecord?

Use `GlideRecordSecure` whenever you're querying or writing data on behalf of the logged-in user and want their actual ACL permissions enforced, such as inside a client-callable Script Include. Use plain `GlideRecord` for trusted server-side logic (like scheduled jobs or Script Includes meant to run with elevated access) where bypassing ACLs is intentional.

### Why validate input on the server if the client-side form already checks it?

Client-side validation is just a UX convenience — a user can call your GlideAjax method directly with arbitrary values via the browser console or an external script, bypassing the form entirely. Server-side validation in the Script Include is the only check that can't be skipped.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=Ccxb4UJKF9c)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/channel/UCMf7pje1x5iZkkEF-Y3PLSA)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← REST API Integration — Part 2](01-rest-api-2.md) · [Call a Script Include in a Flow Designer Custom Action →](03-flow-designer-custom-action.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
