# Configure Your PDI for Development

_Part of Module 0 · Getting Started & Your PDI · Getting Started & Your PDI · [ServiceNow Developer Course](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)_

**Estimated time:** 20 min

## What you'll learn

By the end of this topic your instance will be configured for real development work: debugging on, logs visible, a dedicated scoped lab application created, and a verification script proving everything works.

## Watch

{% embed url="https://www.youtube.com/watch?v=Gis8R0Jv3sw" %}

[Watch on YouTube](https://www.youtube.com/watch?v=Gis8R0Jv3sw)

## Overview

A brand-new PDI works, but it is not yet set up for a developer. In this topic you turn on the tools that let you **see what your code is doing**, and you create the scoped application that will hold all of your course work in one clean, exportable place.

## Key concepts

- **Scripts - Background** — a page where you run server-side JavaScript instantly, without building anything. Your primary sandbox for Modules 1, 3, and 4.
- **Session debugging** — turns on client-side and server-side visibility so you can see logs, field changes, and Business Rule execution.
- **Script log statements** — where `gs.info()` output from server scripts appears.
- **Global vs scoped** — Global is the shared, unnamespaced application. A **scoped application** namespaces your work (e.g. `x_course_lab`), isolates it, and lets you export it cleanly. You will build in scope, like a professional.
- **Update sets vs scoped app export** — both move work between instances. You export your scoped app so your lab survives a PDI reset.

## Step-by-step

### 1. Find Scripts - Background

1. In the navigation filter, type `Scripts - Background`.
2. Open **System Definition → Scripts - Background**.
3. Keep this tab handy — you will use it constantly.

### 2. Turn on developer visibility

1. In the navigation filter, search **System Diagnostics → Session Debug**.
2. Enable **Debug All** — or, for less noise, enable **JavaScript Log and Field Watcher** plus **Debug Business Rule**.
3. Open **System Logs → System Log → Script Log Statements**. This is where your `gs.info()` output will land.

{% hint style="info" %}
Turn Debug All off again when you are done with a session — it clutters the UI and slows the instance down.
{% endhint %}

### 3. Create your lab application

1. Open **Studio** (search "Studio" in the navigation filter, or **All → Studio**).
2. Click **Create Application → Start from scratch**.
3. Set the name to `Course Lab` and confirm the scope name reads `x_course_lab` (your prefix may differ — that is fine, just note it).
4. Create the application.

Everything you build in this course goes in this scoped app. That keeps your work organised, namespaced, and exportable.

### 4. Verify the whole setup with one script

Open **Scripts - Background** and run:

```javascript
// 1. Confirm scripts execute and identify the session
gs.info('PDI is working. Current user: ' + gs.getUserName());
gs.info('Instance release: ' + gs.getProperty('glide.buildname'));

// 2. Confirm you can query data
var gr = new GlideRecord('incident');
gr.setLimit(1);
gr.query();
if (gr.next()) {
  gs.info('First incident number: ' + gr.getValue('number'));
} else {
  gs.info('No incidents found — sample data may not be loaded.');
}
```

Now open **System Logs → System Log → Script Log Statements**. You should see all three messages. If you do, your environment is ready for the rest of the course.

### 5. Export your work (do this at the end of every session)

1. In Studio, open your `Course Lab` application.
2. Use **File → Export Application** (or publish it to an update set).
3. Keep the export file. If your PDI is ever reset or reclaimed, you can restore your work into a new instance in minutes.

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

## Troubleshooting

| Problem | Fix |
| --- | --- |
| No log output | Confirm you ran the script in **Scripts - Background**, and refresh Script Log Statements. |
| Cannot open Studio | Confirm you are logged in as the admin user, not a sample user like Abel Tuter. |
| Scope name rejected | Prefixes are per-instance. Accept the one offered and note it — use it wherever the course says `x_course_lab`. |
| Debug output overwhelming | Turn off **Debug All** and enable only JavaScript Log and Field Watcher. |

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

---
<!--NAV-->
[← Create Your Personal Developer Instance (PDI)](02-create-your-pdi.md) · [JavaScript Essentials for ServiceNow →](../module-1-foundations/00-javascript-essentials.md)
