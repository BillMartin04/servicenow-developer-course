---
description: "GlideSystem (gs) is your gateway to server-side utilities: logging, users, dates, properties, and messages."
---

# Understanding GlideSystem for Efficient Scripting

**Quick answer:** GlideSystem (gs) is your gateway to server-side utilities: logging, users, dates, properties, and messages. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=q1OfAmqvMo4" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/q1OfAmqvMo4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to use gs for logging, users, dates, and properties.

## Overview

GlideSystem (gs) is your gateway to server-side utilities: logging, users, dates, properties, and messages.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=q1OfAmqvMo4

## Key concepts

- **gs logging methods** — `gs.info()`, `gs.warn()`, and `gs.error()` write to the system log (`syslog`) with different severity levels, so you can filter noisy info messages from real errors.
- **User and role checks** — `gs.getUserID()`, `gs.getUserName()`, and `gs.getUser().getName()` return details about the session's current user, while `gs.hasRole('admin')` checks role membership for access decisions.
- **Date/time helpers** — `gs.now()`, `gs.nowDateTime()`, and `gs.daysAgo()` return GlideSystem-formatted date strings in the instance's time zone, sparing you from manual `Date` object handling.
- **System properties and messages** — `gs.getProperty('key')` reads a `sys_properties` value at runtime, and `gs.addInfoMessage()` / `gs.addErrorMessage()` surface a message banner to the user on the next page load.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Use gs to log a message that mixes logging, user info, dates, and a system property.

- [ ] In Scripts - Background, get the current user's name with `gs.getUserName()`
- [ ] Get the current date/time with `gs.nowDateTime()`
- [ ] Read a known property, e.g. `gs.getProperty('glide.servlet.uri')`
- [ ] Combine all three into one `gs.info()` message
- [ ] Change the message to `gs.error()` and confirm it appears with error severity in the System Log

**Done when:** your log entry shows the current user, the current date/time, and the property value on one line, and you can see the severity change between the `gs.info()` and `gs.error()` entries in **System Logs > System Log**.

## Frequently asked questions

### What's the difference between gs.info(), gs.warn(), and gs.error()?

They write to the same system log but tag entries with different severity levels, which controls how they're filtered and highlighted in **System Logs > System Log**. Use `gs.error()` only for genuine failures you want to stand out during triage, and `gs.info()` for routine trace output during development.

### Why does gs.getUser() sometimes return unexpected values in a background script?

Background scripts run as whichever user is currently logged in in the UI session you launched them from, so `gs.getUser()` reflects that session, not necessarily an admin or the record's owner. If you need consistent, user-independent behavior, don't rely on the current session's identity in scheduled jobs or Script Includes meant to run as system.

### When should I use a system property instead of hardcoding a value?

Use `gs.getProperty()` whenever a value might change between environments (dev, test, prod) or needs to be adjustable without a code change, such as a threshold, email address, or feature flag. Hardcoded values force you to edit and re-test scripts every time the value changes, while a `sys_properties` record can be updated by an admin instantly.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=q1OfAmqvMo4)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/@techtalkwithbill)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← Scripting in Catalog UI Policies](../module-2-client-side/03-catalog-ui-policies.md) · [GlideRecord Practical Demo — Part 1 →](01-gliderecord-1.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
