# Where Scripts Run: Client vs Server

_Part of Module 1 · JavaScript & Scripting Foundations · JavaScript & Scripting Foundations · [ServiceNow Developer Course](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)_

## Watch

{% hint style="info" %}
**Video to record.** The written lesson below is complete and can be followed on its own.
{% endhint %}

## Overview

The single most important mental model in ServiceNow development is knowing **where your code runs** — in the user's browser (client) or on the ServiceNow server. Get this right and everything about performance, security, and API choice falls into place. Get it wrong and you'll write slow, insecure, or simply non-functional code.

## The two worlds

| | Client-side | Server-side |
| --- | --- | --- |
| **Runs on** | The user's browser | The ServiceNow application node |
| **Language** | Modern JavaScript + DOM-ish APIs | JavaScript (ES5 baseline via Rhino) |
| **Main API** | `g_form`, `g_user`, `g_scratchpad` | `GlideRecord`, `GlideSystem (gs)`, Script Includes |
| **Sees the database?** | ❌ Not directly | ✅ Yes |
| **Sees the form fields?** | ✅ Yes, instantly | ❌ Only via submitted data |
| **Blocks the user while running?** | ✅ Yes (keep it fast) | ❌ No (but adds server load) |

## Key concepts

### Client-side scripts

Run in the browser and react to what the user does on a form — without a round-trip to the server.

- **Client Scripts** (onLoad, onChange, onSubmit, onCellEdit)
- **UI Policies** (declarative field behaviour)
- **Catalog Client Scripts / Catalog UI Policies**

They use `g_form` to read and manipulate fields, `g_user` to check the current user's roles, and `g_scratchpad` to read data the server pushed down at load time. **They cannot query the database directly** — for that they must call the server.

### Server-side scripts

Run on the ServiceNow node, with full database access.

- **Business Rules** (before, after, async, display)
- **Script Includes** (reusable server classes)
- **Scheduled Jobs, Flow actions, REST resources**

They use `GlideRecord` to read/write data and `gs` for utilities. They never see the live form — only the record as it exists (or is being submitted).

### The bridge: GlideAjax

When a client script needs server data (e.g. "is this username taken?"), it calls a **client-callable Script Include** via **GlideAjax**. This is the sanctioned bridge between the two worlds. You'll build one in [Module 4](../module-4-script-includes-glideajax/01-glideajax.md).

## A form's lifecycle (worked example)

1. User opens an Incident form.
2. **Server** runs any **display Business Rules** — these can stash data into `g_scratchpad`.
3. The form HTML is sent to the browser.
4. **Client** runs **onLoad** client scripts and applies **UI Policies**.
5. User edits a field → **onChange** client scripts fire (still in the browser).
6. Need server data mid-edit? → **GlideAjax** call to a Script Include.
7. User clicks Save → **onSubmit** client scripts run, then the record is sent to the server.
8. **Server** runs **before** Business Rules → database write → **after** Business Rules → **async** Business Rules queued.

## Decision guide

- **Only manipulating visible form fields?** → Client-side (fast, no server load).
- **Need database data or must enforce a rule reliably?** → Server-side (a determined user can bypass client scripts).
- **Client needs server data without a full save?** → GlideAjax.
- **Security or data integrity matters?** → Never trust client-side alone; enforce on the server.

{% hint style="warning" %}
Client scripts are for **user experience**, not security. Anything that *must* be enforced (validation, access, data rules) belongs on the server, because client code can be bypassed.
{% endhint %}

## Hands-on

{% hint style="success" %}
Do these in your PDI to feel the difference.
{% endhint %}

- [ ] Open an Incident form and, in the browser console, run `g_form.getValue('short_description')` — note the client can read fields instantly
- [ ] In **Scripts - Background** (server), query the same incident with GlideRecord — note the server reads the database, not the form
- [ ] For each script type in this course, write down whether it is client or server
- [ ] Trace one form save end-to-end using the lifecycle above

**Done when:** you can state, for every script type in this course, whether it runs client-side or server-side, and you have seen the browser read a field instantly while the server read the same record from the database.

## Frequently asked questions

### Why can't a client script just query the database directly?

Client scripts run in the user's browser, which has no direct connection to the database — only the ServiceNow node does. To get server data mid-form, the client must call a client-callable Script Include through **GlideAjax**. This boundary is a security feature, not a limitation: it keeps data access on the server where ACLs are enforced.

### If I validate a field in an onSubmit client script, is that enough?

No. Client-side validation is for **user experience** — fast feedback — but it can be bypassed (disabled JavaScript, direct API calls, list edits). Any rule that *must* hold has to be enforced server-side as well, typically in a **before Business Rule** or a data policy. Treat client validation as a convenience layer on top of server enforcement.

### What is `g_scratchpad` and when would I use it?

`g_scratchpad` is a small object the **server** populates in a **display Business Rule** before the form renders, and the **client** reads on load. Use it to hand a client script data it would otherwise need a GlideAjax round-trip to fetch — for example a config flag or a related record's value — saving a call when you already know the data at render time.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the course videos and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/channel/UCMf7pje1x5iZkkEF-Y3PLSA)

## Resources

- [ServiceNow: Client vs server scripting](https://www.servicenow.com/docs/)
- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/channel/UCMf7pje1x5iZkkEF-Y3PLSA)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← JavaScript Essentials for ServiceNow](00-javascript-essentials.md) · [Mastering Client Scripts for Beginners →](../module-2-client-side/00-client-scripts.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
