---
description: "Part 2 shows a fuller DI implementation and a simple service container pattern."
---

# Dependency Injection — Part 2

**Quick answer:** Part 2 shows a fuller DI implementation and a simple service container pattern. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=wpSaSDFX4Nc" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/wpSaSDFX4Nc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to build a simple service container pattern.

## Overview

Part 2 shows a fuller DI implementation and a simple service container pattern.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=wpSaSDFX4Nc

## Key concepts

- **Substituting a fake dependency** — because `initialize(dependency)` accepts any object with the right method shape, you can pass in a plain object with a stubbed method (e.g. `{ getActiveCount: function() { return 5; } }`) instead of the real repository when you want predictable output.
- **Verifying behaviour with a fake** — a fake lets you assert on the class's own logic (formatting, branching, thresholds) in isolation, because the fake's return value is fixed and known, unlike a live `GlideRecord` query whose result depends on current data.
- **When a fake is worth the extra code** — write one when the real dependency is slow, has side effects (sends an email, calls an external API), or the input data is hard to set up reliably; skip it for simple, cheap operations where hitting the real dependency in Scripts - Background is just as easy.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Substitute a fake `IncidentRepository` into `IncidentNotifier` from the previous lesson and verify its formatting logic.

- [ ] Reuse the `IncidentNotifier` Script Include with `initialize(repository)` from Part 1
- [ ] In Scripts - Background, define a fake object `var fakeRepo = { getActiveCount: function() { return 42; } };`
- [ ] Call `new IncidentNotifier(fakeRepo).getSummary()` and log the result
- [ ] Confirm the summary text correctly embeds the number 42 without querying the real Incident table
- [ ] Change the fake to return `0` and confirm the summary text changes appropriately (e.g. a "no active incidents" phrasing if your logic branches on zero)

**Done when:** the logged summary always reflects exactly what the fake returns, proving `IncidentNotifier`'s formatting logic works independently of real incident data.

## Frequently asked questions

### What exactly counts as a "fake" dependency in ServiceNow scripting?

A fake is any plain JavaScript object or lightweight class that implements the same method names the real dependency exposes, but returns fixed, predictable values instead of querying the database. It doesn't need to be a formal mocking library — a simple object literal with stub methods is usually enough.

### Why not just test against real data in the Incident table instead of faking it?

Real data changes over time, so a test that passes today might fail tomorrow simply because incidents were closed or created. A fake keeps the input fixed, so you're only verifying your class's own logic, not the current state of the table.

### Do I need to change the class under test to support fakes?

No, provided the class already accepts its dependency through `initialize()` as covered in Part 1. If a dependency is instead hardcoded with `new` inside a method, you'd need to refactor it to accept an injected value first before a fake can be substituted.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=wpSaSDFX4Nc)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/@techtalkwithbill)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← How to Implement Dependency Injection — Part 1](01-dependency-injection-1.md) · [Script Include & GlideAjax for Architects →](03-script-include-glideajax-arch.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
