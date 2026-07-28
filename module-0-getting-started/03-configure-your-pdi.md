---
description: "A brand-new PDI works, but it is not yet set up for a developer."
---

# Configure Your PDI for Development

**Quick answer:** A brand-new PDI works, but it is not yet set up for a developer. In this topic you turn on the tools that let you **see what your code is doing**, and you create the scoped application that will hold all of your course work in one clean, exportable place. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=Gis8R0Jv3sw" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/Gis8R0Jv3sw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this topic your instance will be configured for real development work: debugging on, logs visible, a dedicated scoped lab application created, and a verification script proving everything works.

## Overview

A brand-new PDI works, but it is not yet set up for a developer. In this topic you turn on the tools that let you **see what your code is doing**, and you create the scoped application that will hold all of your course work in one clean, exportable place.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=Gis8R0Jv3sw

## Key concepts

- **Scripts - Background** — a page where you run server-side JavaScript instantly, without building anything. Your primary sandbox for Modules 1, 3, and 4.
- **Session debugging** — turns on client-side and server-side visibility so you can see logs, field changes, and Business Rule execution.
- **Script log statements** — where `gs.info()` output from server scripts appears.
- **Global vs scoped** — Global is the shared, unnamespaced application. A **scoped application** namespaces your work (e.g. `x_course_lab`), isolates it, and lets you export it cleanly. You will build in scope, like a professional.
- **Update sets vs scoped app export** — both move work between instances. You export your scoped app so your lab survives a PDI reset.

## Hands-on

{% hint style="success" %}
This is the setup checklist the entire rest of the course depends on.
{% endhint %}

- [ ] Open **Scripts - Background** and bookmark it
- [ ] Enable session debugging (JavaScript Log and Field Watcher at minimum)
- [ ] Open **Script Log Statements** and bookmark it
- [ ] Create the `Course Lab` scoped application in Studio
- [ ] Run the verification script and confirm all three log lines appear
- [ ] Export your scoped app once, so you know how

## Frequently asked questions

### Why can't I see my gs.info() output anywhere?

Most likely Script Log Statements isn't open, or you're looking in the wrong place. Open the **Script Log Statements** module (or System Logs > Script Log Statements) and re-run your script — `gs.info()` output appears there, not in the browser console, since it runs server-side.

### Should I build my course work in the Global scope or a scoped app?

Always use the scoped application (`x_course_lab`) you create in this lesson. Global is unnamespaced and shared, so it's easy to create naming collisions and hard to export cleanly; a scoped app isolates your work and lets you move or back it up with a single export.

### What's the difference between exporting a scoped app and using an update set?

Both move customizations between instances, but a scoped app export bundles the entire application (all its tables, scripts, and records) as one package, while update sets track individual changes and are typically used for smaller, incremental promotions between instances. For this course, exporting the whole scoped app is simpler and safer against a PDI reset.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=Gis8R0Jv3sw)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← Create Your Personal Developer Instance (PDI)](02-create-your-pdi.md) · [JavaScript Essentials for ServiceNow →](../module-1-foundations/00-javascript-essentials.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
