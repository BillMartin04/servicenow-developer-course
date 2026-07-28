# Course Syllabus

This page is the complete map of the curriculum: what every module covers, why it sits where it does, and what you will be able to build once you finish it. Read it alongside the [Course Objectives](objectives.md) — objectives state the destination, this page states the route.

The course is built one page per video. Each topic page embeds its video, lists the key concepts it covers, and ends with a hands-on task you complete in your own Personal Developer Instance (PDI) before moving to the next topic. Nothing is theoretical for long — you are either watching, reading, or typing code.

**Total instruction:** ~6 hours 45 minutes · **Final exercise:** 4–6 hours · **Suggested duration:** 7 weeks self-paced

## How this curriculum is sequenced

The eight modules are not an arbitrary playlist order — they are a dependency chain. Module 0 sets up the PDI before any code is written, because every later hands-on task assumes a live, configured instance; skipping it means every subsequent exercise stalls on environment problems instead of teaching the intended concept. Module 1 then builds the JavaScript subset ServiceNow actually uses and the single mental model that governs everything after it: where a given piece of code executes. You cannot make sensible decisions about Client Scripts, Business Rules, or Script Includes until you can look at a script and say with certainty whether it runs in the browser or on the server.

From there the course moves outward in layers that mirror how a real transaction flows through the platform: client side first (Module 2, because the form is what a user actually touches), then server side (Module 3, because that is where data is authoritative), then reusable server logic (Module 4, because duplicated server code is the first thing that breaks a growing application), then architecture (Module 5, because you cannot design good boundaries between components until you have components worth bounding), then integration, security, and reliability (Module 6, because talking to the outside world safely is only possible once you already have layers to isolate the risk), and finally a graded build (Module 7) that forces every earlier skill to work together at once. Each module is a prerequisite for the one that follows: you cannot judge whether a Business Rule is well-written until you can read the GlideRecord calls inside it, and you cannot design a service layer until you can already write a Script Include that layer will live in.

Two points in the sequence deliberately break from what a certification-style curriculum might do, and both are called out on the page with an info hint. In Module 3, GlideRecord is taught before Business Rules, even though Business Rules are usually presented first in ServiceNow training. The reason is simple: a Business Rule script is mostly GlideRecord code wrapped in a trigger. Teaching the trigger mechanics before the API leaves learners able to recite "before, after, async, display" without being able to write a single line inside any of them. Learning the API first makes the automation topic a small addition instead of the whole lesson.

In Module 5, layering, dependency injection, and the Script Include/GlideAjax architecture pattern are taught before queueing and the service bus, even though asynchronous processing is often treated as the more advanced topic. The reason is again ordering by dependency: you must be able to separate a controller from a service from a data-access layer before you can safely decouple those layers with an event queue, and a service bus only makes sense once you already have a service worth exposing through one. Distributing badly-separated logic just moves the mess further away from where it was created.

{% hint style="info" %}
**The two re-sequencing decisions on this page:** GlideRecord before Business Rules in Module 3 (the automation topic depends on the API), and layering/dependency injection before queueing/service bus in Module 5 (you must separate concerns before you can distribute them).
{% endhint %}

This sequencing also explains why the course does not offer a "skip ahead" track, even for learners who already know parts of ServiceNow. An experienced admin might reasonably feel Module 2 is beneath them, but Module 7's rubric grades client-side quality as part of a single integrated build, not as an isolated skill — so a gap in Module 2 resurfaces as lost points in the final exercise, not as an obviously missing topic along the way. The [Course Objectives](objectives.md) page includes a table matching background to entry point for exactly this reason: a working ServiceNow admin can move faster through Modules 0–2, but should not skip them outright, and an experienced developer from another platform can move fastest through Modules 0–4 while still needing Modules 5–7 for the architecture and assessment this course is actually built around.

## Curriculum at a glance

| Module | Title | Focus | Topics | Time |
| --- | --- | --- | --- | --- |
| [0](module-0-getting-started/README.md) | Getting Started & Your PDI | Orientation, instance setup | 4 | ~55 min |
| [1](module-1-foundations/README.md) | JavaScript & Scripting Foundations | JS subset, client vs server | 2 | ~40 min |
| [2](module-2-client-side/README.md) | Client-Side Scripting | Forms, UI Policies, catalog | 4 | ~45 min |
| [3](module-3-server-side/README.md) | Server-Side Scripting | GlideSystem, GlideRecord, Business Rules | 7 | ~1 hr 20 min |
| [4](module-4-script-includes-glideajax/README.md) | Script Includes & GlideAjax | Reusable server code, client bridge | 4 | ~45 min |
| [5](module-5-architecture/README.md) | Architecture & Design Patterns | OOP, DI, events, service bus | 6 | ~1 hr 15 min |
| [6](module-6-integrations/README.md) | Integrations, Security & Reliability | REST, secure coding, error handling | 5 | ~1 hr |
| [7](module-7-capstone/README.md) | Final Exercise & Assessment | Project, rubric, architect roadmap | 5 | ~5–7 hrs |

**Legend:** 🎬 video topic · 📖 written guide (video not yet recorded)

Read the table top to bottom the first time through — the Time column is a planning tool, not a target to beat. A topic marked 12 minutes of video usually costs another 15–30 minutes once you factor in the hands-on task on the same page, and Modules 3 and 5 in particular reward slowing down rather than speeding up, because the concepts they introduce (the GlideRecord API, dependency injection) are reused, unannounced, in every module that follows them. The 📖 icon marks topics that are currently a written guide rather than a recorded video — the content and hands-on task are complete either way, and the video is added to the page later without changing the URL or the topic number.

---

## Module 0 · Getting Started and Your PDI

**Goal:** understand the role, then get a working development environment **before writing any code**.

This module has two jobs, and it deliberately does them in this order. First it answers "what does a ServiceNow developer actually do, and is this course for me" — because a learner who does not know where the skills ladder leads has no way to judge whether an exercise three modules from now is worth the effort. Second, and more importantly, it gets you a live, correctly configured Personal Developer Instance. Every single hands-on task from Module 1 onwards assumes this instance exists and is set up a specific way: debugging on, script logs visible, and a scoped application ready to build in. Skipping the configuration step is the single most common reason learners get stuck early — not because the scripting is hard, but because they are trying to run it somewhere that is not ready.

The decision to build inside a scoped application (`x_course_lab`) rather than in Global is intentional and carries through the entire course. Scoping isolates your lab work, makes it exportable, and mirrors how professional ServiceNow development actually happens — nobody builds production applications unscoped. This is also your insurance policy: PDIs hibernate after roughly ten days of inactivity and can eventually be reclaimed, so the module teaches you to export your scoped app precisely so a reclaimed instance costs you a login, not your work.

There is no coding knowledge assumed here, and there should not be — this is the only module where the goal is orientation and plumbing rather than a scripting skill. Everything that follows treats a working PDI as a given: Module 1's first hands-on task runs inside Scripts - Background on this instance, and the final exercise in Module 7 is built inside the exact `x_course_lab` application you create here. If you ever need to start over, the module you rebuild from is this one.

**What you'll be able to do**
- Describe the ServiceNow developer role and the skills ladder this course climbs
- Request, log into, and wake a free Personal Developer Instance from the Developer Portal
- Enable session debugging and locate `gs.info()` output in the script log
- Create a scoped lab application (`x_course_lab`) instead of building in Global
- Run a verification script from Scripts - Background and confirm it executes correctly
- Explain how to export your work so a PDI reset or reclamation does not cost you progress

**Prerequisites:** None — this is the starting point.

**Key APIs & concepts:** Personal Developer Instance (PDI), Scripts - Background, `gs.info()`, session debugging, scoped application (`x_course_lab`), update sets, scoped app export.

| # | Topic | Type | Time | What it covers |
| --- | --- | --- | --- | --- |
| 0.1 | [How to Become a ServiceNow Developer](module-0-getting-started/00-how-to-become-a-developer.md) | 🎬 | 12 min | Maps the skills ladder from admin fundamentals through scripting to architecture, and the career paths (developer, consultant, architect) it opens up. |
| 0.2 | [Meet Your Instructor & Kickstart Your Career](module-0-getting-started/01-meet-your-instructor.md) | 🎬 | 8 min | Introduces the instructor's "build it the right way" philosophy and clarifies who the course is written for. |
| 0.3 | [Create Your Personal Developer Instance (PDI)](module-0-getting-started/02-create-your-pdi.md) | 🎬 | 15 min | Requests a free PDI from the Developer Portal and explains hibernation, reclamation, reset, and choosing the newest release family. |
| 0.4 | [Configure Your PDI for Development](module-0-getting-started/03-configure-your-pdi.md) | 🎬 | 20 min | Turns on session debugging and script logging, explains Global vs scoped applications, and has you create the `x_course_lab` scoped app with a passing verification script. |

**Common pitfalls**
- Building lab exercises in Global instead of the `x_course_lab` scoped app, which makes later exports messy or impossible.
- Never enabling session debugging, then being unable to see why a Business Rule or client script did not fire in later modules.
- Letting the PDI hibernate without knowing how to wake it, and assuming lost access means lost work.

[Open module →](module-0-getting-started/README.md)

---

## Module 1 · JavaScript and Scripting Foundations

**Goal:** write correct JavaScript and know exactly where each script executes.

This is the shortest module in the course by topic count, but arguably the one everything else depends on most directly. Topic 1.1 covers the ServiceNow-relevant subset of JavaScript — variables and types, truthiness, function scope and closures, objects and arrays, iteration, and `JSON.stringify`/`JSON.parse` for debugging. None of this is exotic JavaScript; it is deliberately the boring, foundational subset, because ServiceNow scripting bugs are disproportionately caused by misunderstanding truthiness (`'0'` is truthy, `0` is not) or `var` scoping rather than by anything platform-specific.

Topic 1.2 is the conceptual core of the whole course: the client/server execution model. Client-side code (Client Scripts, UI Policies, catalog variants) runs in the browser, uses `g_form`, `g_user`, and `g_scratchpad`, and cannot query the database directly. Server-side code (Business Rules, Script Includes, Scheduled Jobs, Flow actions) runs on the ServiceNow node, uses `GlideRecord` and `gs`, and never sees the live form. GlideAjax is introduced here as the sanctioned bridge between the two — a concept this module only names, because building one requires Script Includes, which is Module 4.

Every later module assumes you can look at an unfamiliar script and immediately classify it as client-side or server-side without needing to be told. Getting this wrong is the root cause of a huge share of real-world ServiceNow bugs — for example, trying to query a table from a Client Script, or expecting `g_form` inside a Business Rule. This module has no table of its own to build against — unlike every module from here on, its hands-on work is conceptual rather than a specific artefact — but Module 2 begins applying it to a real form within the hour, and every later module's "Prerequisites" line traces back to the classification skill built here.

**What you'll be able to do**
- Write JavaScript using the ES5-safe subset ServiceNow scripting supports
- Identify falsy values correctly and avoid truthiness bugs like treating `'0'` as false
- Use closures and function scope without introducing the classic loop-variable bug
- Classify any given script as client-side or server-side purely from the APIs it uses
- Trace a form's request lifecycle from server render to browser interaction and back
- Explain why GlideAjax exists as the bridge between the two execution contexts

**Prerequisites:** Module 0 complete with a working, configured PDI.

**Key APIs & concepts:** `var`, truthiness, closures, `JSON.stringify`/`JSON.parse`, `g_form`, `g_user`, `g_scratchpad`, `GlideRecord`, `gs`, GlideAjax (introduced, built in Module 4).

| # | Topic | Type | Time | What it covers |
| --- | --- | --- | --- | --- |
| 1.1 | [JavaScript Essentials for ServiceNow](module-1-foundations/00-javascript-essentials.md) | 📖 | 20 min | Covers variable types, truthiness pitfalls, function scope and closures, object/array manipulation, iteration patterns, and JSON serialisation for debugging. |
| 1.2 | [Where Scripts Run: Client vs Server](module-1-foundations/01-where-scripts-run.md) | 📖 | 20 min | Contrasts client-side APIs (`g_form`, `g_user`, `g_scratchpad`) with server-side APIs (`GlideRecord`, `gs`), and introduces GlideAjax as the bridge between them. |

**Common pitfalls**
- Treating the string `'0'` or `'false'` as falsy, causing conditional logic to run when it should not.
- Assuming a Client Script can read the database directly instead of routing the request through GlideAjax.
- Writing `var` inside a loop and expecting block-scoping behaviour that ServiceNow's JavaScript engine does not provide.

[Open module →](module-1-foundations/README.md)

---

## Module 2 · Client-Side Scripting

**Goal:** control form behaviour in the browser without hurting performance.

With the execution model established in Module 1, this module puts it to work on the surface users actually see: the form. Client Scripts (onLoad, onChange, onSubmit, onCellEdit) are the imperative option — you write the logic. UI Policies are the declarative option — you set conditions and actions and let the platform apply them. Learning both, and more importantly learning when to reach for which, is the practical skill here; a UI Policy that could be built declaratively but is instead hand-coded in a Client Script is harder to maintain and easier to break.

The module then extends the same two concepts into the Service Catalog: Catalog Client Scripts and Catalog UI Policies operate on catalog variables rather than table fields, and behave subtly differently from their standard counterparts — a distinction that trips up developers moving from form scripting to catalog work for the first time. This matters well beyond the classroom: the Module 7 final exercise's user-facing intake form is a catalog item, and its client-side validation is graded directly against what this module teaches.

The throughline across all four topics is performance discipline. Client-side code runs in every user's browser on every interaction, so a slow or synchronous client script is felt platform-wide, not just by its author. This module is where good habits — asynchronous calls, minimal DOM manipulation, restraint about what actually needs to run onChange — are formed before Module 4 introduces GlideAjax and gives you the power to call the server from a client script badly.

This module also has a direct line into the final exercise's rubric: criterion 8, client-side quality & UX, is graded specifically on whether the catalog client script you build in Module 7 uses asynchronous GlideAjax with clear user feedback and no perceptible form delay — the exact discipline this module exists to build.

**What you'll be able to do**
- Build onLoad, onChange, onSubmit, and onCellEdit Client Scripts correctly
- Choose a UI Policy over a scripted Client Script when the requirement is purely declarative
- Control field visibility, mandatory state, and read-only state without server round-trips
- Apply Catalog Client Scripts and Catalog UI Policies to Service Catalog variables
- Recognise the practical differences between standard and catalog-context `g_form` usage
- Keep client-side scripts fast enough that users never notice them running

**Prerequisites:** Modules 0–1, especially comfort with `g_form` and the client/server execution model.

**Key APIs & concepts:** `g_form`, `g_user`, onLoad/onChange/onSubmit/onCellEdit, UI Policy conditions and actions, reverse-if-false, Catalog Client Scripts, Catalog UI Policies, catalog variables.

| # | Topic | Type | Time | What it covers |
| --- | --- | --- | --- | --- |
| 2.1 | [Mastering Client Scripts for Beginners](module-2-client-side/00-client-scripts.md) | 🎬 | 14 min | Builds all four Client Script types, covers `g_form`/`g_user` usage and onChange field arguments, and sets out performance best practices. |
| 2.2 | [UI Policies Explained](module-2-client-side/01-ui-policies.md) | 🎬 | 12 min | Compares UI Policies with scripted Client Scripts, explains conditions/actions and reverse-if-false behaviour, and when a scripted UI Policy is justified. |
| 2.3 | [Catalog Client Scripts Explained](module-2-client-side/02-catalog-client-scripts.md) | 🎬 | 8 min | Explains catalog client script scope, the variables-vs-fields distinction, and real-world catalog scripting use cases. |
| 2.4 | [Scripting in Catalog UI Policies](module-2-client-side/03-catalog-ui-policies.md) | 🎬 | 9 min | Covers Catalog UI Policy structure, scripting the execute-if-true logic, and applies-on settings for catalog variable sets. |

**Common pitfalls**
- Reaching for a scripted Client Script when a declarative UI Policy would do the same job more maintainably.
- Confusing catalog variable names with table field names when writing catalog client scripts.
- Running expensive logic synchronously on every onChange event, adding perceptible lag to the form.

[Open module →](module-2-client-side/README.md)

---

## Module 3 · Server-Side Scripting

**Goal:** read and write platform data reliably, then automate it with Business Rules.

This is the largest module in the course by topic count, and deliberately so — it is where scripting stops being about the form and starts being about the data underneath it. GlideSystem (`gs`) opens the module because it is the utility layer every subsequent server script leans on: logging with `gs.info()`, checking the current user and roles, working with dates, and reading system properties. It is small enough to learn in one sitting and used constantly afterwards.

The four-part GlideRecord sequence is the technical centre of gravity for the entire course. Part 1 covers the query lifecycle — `addQuery`, `addEncodedQuery`, `next()`, and `get()` by sys_id. Part 2 covers writing: insert, update, and delete, plus why `setValue()` is the disciplined way to change a field. Part 3 extends querying with `orWhere` chaining, `orderBy`, `GlideAggregate`, and dot-walking into related records. Part 4 turns to performance: indexes, `setWorkflow()`, `autoSysFields()`, avoiding nested queries inside loops, and safe bulk operations. Splitting GlideRecord into four parts rather than one long video is intentional — each part is a genuinely separate skill, and rushing straight to performance tuning before you can reliably query and write is how learners develop anti-patterns they never unlearn.

Only once GlideRecord is solid does the module turn to Business Rules — and this is the first of the two deliberate re-sequencing decisions on this page. A Business Rule script is, in practice, mostly GlideRecord code wrapped around a trigger (`current`, `previous`, before/after/async/display timing). Teaching the trigger mechanics first and the API second leaves learners able to name the four timings without being able to write anything meaningful inside any of them. Learning GlideRecord first makes topic 3.6 a short, obvious extension. The module closes with Display Business Rules and `g_scratchpad` — the one sanctioned way to push server-computed data down to the client before the form even renders, foreshadowing the client/server bridge theme that recurs through Module 4.

{% hint style="info" %}
**Why GlideRecord comes before Business Rules:** a Business Rule script is mostly GlideRecord code. Learn the API first and the automation topic becomes obvious instead of overwhelming.
{% endhint %}

The skills built here carry forward more directly than in any other module. Module 4's Script Includes exist largely to hold the GlideRecord logic this module teaches you to write; Module 5's Repository pattern is, mechanically, this module's GlideRecord code moved behind a class boundary; and the final exercise's data access discipline criterion is scored entirely on whether the habits taught in topics 3.2–3.5 — targeted queries, `setLimit()`, no unbounded scans — survived into a real build.

**What you'll be able to do**
- Use GlideSystem (`gs`) for logging, user/role checks, dates, and system properties
- Query records with `addQuery`, `addEncodedQuery`, `next()`, and `get()`
- Insert, update, and delete records safely, using `setValue()` rather than dot-walking blindly
- Use `orWhere`, `orderBy`, `GlideAggregate`, and dot-walking for advanced queries
- Apply GlideRecord performance practices — indexed queries, `setLimit()`, avoiding nested queries in loops
- Write correct before, after, async, and display Business Rules, and explain their order of execution

**Prerequisites:** Modules 0–2, particularly comfort with server-side execution and core JavaScript.

**Key APIs & concepts:** `gs.info()`/`gs.error()`, `GlideRecord`, `addQuery()`, `addEncodedQuery()`, `next()`, `get()`, `insert()`/`update()`/`deleteRecord()`, `setValue()`, `orWhere()`, `orderBy()`, `GlideAggregate`, `setLimit()`, Business Rules (before/after/async/display), `current`/`previous`, `g_scratchpad`.

| # | Topic | Type | Time | What it covers |
| --- | --- | --- | --- | --- |
| 3.1 | [Understanding GlideSystem for Efficient Scripting](module-3-server-side/00-glidesystem.md) | 🎬 | 11 min | Covers `gs` logging methods, user and role checks, date/time helpers, and reading system properties and messages. |
| 3.2 | [GlideRecord Practical Demo — Part 1](module-3-server-side/01-gliderecord-1.md) | 🎬 | 12 min | Builds the GlideRecord query lifecycle: `addQuery`/`addEncodedQuery`, `next()` iteration, and `get()` by sys_id. |
| 3.3 | [GlideRecord Practical Demo — Part 2](module-3-server-side/02-gliderecord-2.md) | 🎬 | 12 min | Covers insert, update, and `deleteRecord`, plus `setValue()` and `setLimit()` for disciplined writes. |
| 3.4 | [GlideRecord Practical Demo — Part 3](module-3-server-side/03-gliderecord-3.md) | 🎬 | 12 min | Adds `orWhere` query chaining, `orderBy`, GlideAggregate basics, and dot-walking to related records. |
| 3.5 | [GlideRecord Practical Demo — Part 4](module-3-server-side/04-gliderecord-4.md) | 🎬 | 12 min | Focuses on query performance and indexes, `setWorkflow`/`autoSysFields`, avoiding nested queries, and safe bulk operations. |
| 3.6 | [How Business Rules Execute (with Examples)](module-3-server-side/05-business-rules-execution.md) | 🎬 | 13 min | Explains the four Business Rule timings, the `current`/`previous` objects, order of execution, and common pitfalls. |
| 3.7 | [Display Business Rules & g_scratchpad](module-3-server-side/06-display-business-rules.md) | 🎬 | 9 min | Covers display rule timing and the `g_scratchpad` pattern for safely passing server data to the client before form load. |

**Common pitfalls**
- Running a GlideRecord query inside a loop instead of batching it, causing severe performance degradation.
- Confusing before and after Business Rules, leading to logic that reads a value before it is actually saved.
- Dot-walking into a field to set it instead of using `setValue()`, silently failing to persist the change.

[Open module →](module-3-server-side/README.md)

---

## Module 4 · Script Includes and GlideAjax

**Goal:** stop repeating yourself — package logic once and call it from anywhere.

By Module 3 you can read and write data and automate it with Business Rules — but if that logic is copied into every Business Rule and Client Script that needs it, the application becomes unmaintainable the moment a rule changes. Module 4 introduces Script Includes as the fix: reusable, correctly-scoped server-side classes that any Business Rule, Script Include, or GlideAjax call can invoke instead of duplicating logic. This is the point in the course where "make it work" starts becoming "make it maintainable."

GlideAjax is taught immediately alongside Script Includes because it is the mechanism that finally makes the Module 1 bridge concept concrete: a client-callable Script Include, extending `AbstractAjaxProcessor`, invoked asynchronously from a Client Script via GlideAjax's `getXMLAnswer`/`getXML` methods. This closes the loop opened in topic 1.2, where GlideAjax was only named. Getting this pattern right — parameters passed correctly, calls made asynchronously with a callback rather than synchronously — is foundational to nearly every later integration in the course, including the final exercise's `FacilitiesController`.

The module closes with `Object.extendsObject`, ServiceNow's inheritance mechanism: the `initialize()` pattern, inheriting behaviour from a parent Script Include, overriding methods, and calling the parent prototype explicitly. This is deliberately placed last in the module, and its two-part split matters — Part 1 establishes the mechanics of inheritance, Part 2 focuses on when it is the wrong tool and composition should be preferred instead. That distinction is a direct precursor to Module 5's dependency injection, where composing collaborators rather than inheriting from them becomes the default professional pattern.

Looking backward, this module is where Modules 1–3 stop being separate skills and start being combined into one artefact: a Script Include is JavaScript (Module 1) that manipulates data (Module 3) and is exposed to the client (the bridge named in Module 1, built here). Looking forward, every Script Include you write from Module 5 onward — `FacilitiesRepository`, `WeatherGateway`, `FacilitiesService`, `FacilitiesController` in the final exercise — is a direct descendant of the pattern taught in topic 4.1.

**What you'll be able to do**
- Build reusable, correctly-scoped Script Includes instead of duplicating logic across Business Rules
- Expose a client-callable Script Include and invoke it asynchronously via GlideAjax
- Pass parameters safely from client to server and return a usable response
- Implement inheritance between Script Includes with `Object.extendsObject`
- Override an inherited method and call the parent prototype explicitly when needed
- Decide when composition is the better choice over inheritance

**Prerequisites:** Modules 0–3. Comfort with server-side scripting and GlideRecord is required.

**Key APIs & concepts:** Script Include, client-callable checkbox, `AbstractAjaxProcessor`, `GlideAjax`, `getXMLAnswer()`, `getXML()`, `Object.extendsObject`, `initialize()`, prototype inheritance, composition vs inheritance.

| # | Topic | Type | Time | What it covers |
| --- | --- | --- | --- | --- |
| 4.1 | [Script Includes Explained (Full Demo)](module-4-script-includes-glideajax/00-script-includes.md) | 🎬 | 13 min | Covers Script Include structure, client-callable vs on-demand configuration, scope and access control, and reuse patterns. |
| 4.2 | [Script Include & GlideAjax](module-4-script-includes-glideajax/01-glideajax.md) | 🎬 | 12 min | Builds `AbstractAjaxProcessor`-based calls with `getXMLAnswer`/`getXML`, parameter passing, and asynchronous best practices. |
| 4.3 | [Script Include & Object.extendObject — Part 1](module-4-script-includes-glideajax/02-extendobject-1.md) | 🎬 | 10 min | Establishes prototype inheritance, the `initialize()` pattern, and how a Script Include inherits behaviour from a parent. |
| 4.4 | [Script Include & Object.extendObject — Part 2](module-4-script-includes-glideajax/03-extendobject-2.md) | 🎬 | 10 min | Covers method overriding, calling the parent prototype, and when to prefer composition over inheritance. |

**Common pitfalls**
- Forgetting to tick "client callable" and then being unable to understand why GlideAjax returns nothing.
- Calling GlideAjax synchronously, blocking the browser while the server responds.
- Reaching for `Object.extendsObject` inheritance when a composed helper object would be simpler and more testable.

[Open module →](module-4-script-includes-glideajax/README.md)

---

## Module 5 · Architecture and Design Patterns

**Goal:** design like an architect — separation of concerns, testability, and scale.

Modules 1–4 taught you to write correct code. Module 5 teaches you to design it — the shift this whole course is built around, and the one that separates a developer from a software architect. It opens with object-oriented principles applied specifically to ServiceNow scripting: encapsulation, cohesion, and SOLID, framed around designing Script Includes that do one job well rather than accumulating unrelated methods over time.

Dependency injection is the technical heart of the module, taught across two parts. Part 1 introduces constructor injection — a Script Include's `initialize()` method accepting its collaborators as arguments instead of instantiating them internally — and explains why that single change is what makes code testable: you can substitute a fake dependency and verify behaviour without touching a real database or a real external API. Part 2 extends this into a simple service container concept for registering and resolving services. Topic 5.4 then applies both OOP and DI to the Script Include/GlideAjax pattern from Module 4, introducing the Repository pattern — isolating GlideRecord access behind a dedicated class — which is exactly the pattern the Module 7 final exercise grades most heavily.

Only after layering and DI are solid does the module turn to queueing, events, and the service bus — the second deliberate re-sequencing on this page. The Event Registry, `gs.eventQueue()`, and Script Actions let you decouple work so a user is not left waiting on something that does not need to happen synchronously; the service bus pattern then generalises this into a reusable integration layer with routing and adapters. Both topics assume you can already separate a controller from a service from a data-access layer — trying to queue or route badly-separated logic just moves the mess asynchronously instead of fixing it. This is also where the course's "AI-ready" framing becomes concrete: layered, injectable, well-bounded code is exactly what makes it safe for an AI coding assistant to extend a codebase without breaking it. It is worth being explicit about what "AI-ready" does not mean here: it is not about prompting a model well, and no topic in this module mentions a specific AI tool. It means building a codebase with clear seams, so that whoever or whatever changes it next — a colleague, a future you, or a coding agent — can reason about the blast radius of a change before making it.

This ordering — layering and dependency injection before queueing and the service bus — is the second re-sequencing decision described earlier on this page, in "How this curriculum is sequenced": clean layers must exist before they can be safely decoupled with events, and a service bus only makes sense once there is a well-bounded service worth exposing through one.

Most learners who struggle in this module are not struggling with the JavaScript — constructor injection is a small syntactic change — they are struggling to see why it matters, because the payoff (testability) is invisible until you actually try to substitute a dependency. The final exercise makes this concrete rather than abstract: Stage 4 requires you to run `FacilitiesService` twice, once against the real `WeatherGateway` and once against a fake one returning a fixed temperature, and rubric criterion 3 is scored directly on whether you can demonstrate that substitution. If Module 5 feels academic while you are in it, it stops feeling that way the moment you sit down to build Module 7.

**What you'll be able to do**
- Apply encapsulation, cohesion, and SOLID principles to Script Include design
- Implement constructor-based dependency injection so a Script Include's collaborators can be substituted for tests
- Build a simple service container to register and resolve dependencies
- Apply the Repository pattern to isolate all GlideRecord access behind one class
- Decide when to fire an event or queue work instead of executing it inline
- Structure a reusable service-bus integration layer with routing and adapters

**Prerequisites:** Modules 0–4. A solid grasp of Script Includes and GlideAjax is essential.

**Key APIs & concepts:** SOLID principles, encapsulation, `initialize(dependency)` constructor injection, service container, Repository pattern, `gs.eventQueue()`, Event Registry, Script Actions, service bus, adapters.

| # | Topic | Type | Time | What it covers |
| --- | --- | --- | --- | --- |
| 5.1 | [Object-Oriented Principles for Architects](module-5-architecture/00-oop-principles.md) | 🎬 | 14 min | Applies encapsulation, cohesion, and SOLID principles to designing maintainable Script Includes. |
| 5.2 | [How to Implement Dependency Injection — Part 1](module-5-architecture/01-dependency-injection-1.md) | 🎬 | 12 min | Introduces what DI is and why it matters, and implements constructor injection in a Script Include to decouple dependencies. |
| 5.3 | [Dependency Injection — Part 2](module-5-architecture/02-dependency-injection-2.md) | 🎬 | 12 min | Builds a simple service container concept for registering and resolving injected services. |
| 5.4 | [Script Include & GlideAjax for Architects](module-5-architecture/03-script-include-glideajax-arch.md) | 🎬 | 13 min | Applies layering and separation of concerns to the Script Include/GlideAjax pattern and introduces the Repository pattern. |
| 5.5 | [Queueing & Event-Driven Architecture](module-5-architecture/04-queueing-event-driven.md) | 🎬 | 13 min | Covers the Event Registry, `gs.eventQueue()`, Script Actions, and async processing patterns to decouple work. |
| 5.6 | [Service Bus Architecture for Reusable Integrations](module-5-architecture/05-service-bus.md) | 🎬 | 12 min | Designs a scalable, reusable integration layer using service bus concepts, routing, and adapters. |

**Common pitfalls**
- Instantiating dependencies directly inside a method instead of injecting them, making the class impossible to test in isolation.
- Letting GlideRecord calls leak outside the repository class "just this once."
- Reaching for an event queue to fix a design problem that layering should have solved first.

[Open module →](module-5-architecture/README.md)

---

## Module 6 · Integrations, Security and Reliability

**Goal:** talk to the outside world safely, and fail predictably when it breaks.

Module 5 gave you clean layers; Module 6 puts a layer at risk — an external system you do not control — and teaches you to contain that risk instead of letting it leak into the rest of the application. REST integration is taught across two parts: Part 1 covers `RESTMessageV2` basics — endpoints, methods, headers, and parsing JSON responses — and Part 2 adds authentication (basic/OAuth), timeouts, retries, and wrapping the whole thing in a reusable integration Script Include. The instinct this module builds is to never call `RESTMessageV2` directly from a Business Rule or Client Script; it belongs behind a gateway class, exactly as Module 5's layering taught.

Secure coding follows immediately, because an integration layer that is not secured is a bigger liability than no integration at all. This topic covers ACLs, input validation, the risks of exposing a client-callable Script Include too broadly, and `GlideRecordSecure` for enforcing access control on user-context reads and writes. The central lesson — that client-side validation is a convenience, not a control, and every input must be revalidated server-side — is one of the most heavily weighted items in the Module 7 rubric.

Flow Designer custom actions then bridge low-code and pro-code: calling a Script Include from a Flow action's script step, with defined inputs and outputs, lets citizen developers use logic that professional developers wrote and tested. The module closes with global error handling — a centralised `GlobalErrorHandler` Script Include that every layer routes its try/catch blocks through, so failures are logged consistently with layer, method, and record context instead of being swallowed silently or left to throw an unhandled stack trace. This is the single most reused pattern in the Module 7 final exercise: every layer of the Weather-Aware Facilities Requests build (controller, service, gateway, repository) is expected to call it.

Module 6 is where the course's "AI-ready" framing stops being a slogan and becomes a checklist. Secure-by-default server-side validation, isolated integration gateways, and consistent centralised error handling are precisely the properties that let a human reviewer — or an AI coding assistant — change one part of an application with confidence that a fault will surface clearly rather than corrupt silently elsewhere. Everything taught here is graded directly in the final exercise's integration robustness, security, and error handling criteria, worth 30 of the 100 rubric points combined.

**What you'll be able to do**
- Call an external REST API using `RESTMessageV2` wrapped inside a dedicated gateway Script Include
- Handle authentication, timeouts, and non-200 responses on an integration without letting failures propagate raw
- Validate and enforce access control server-side, never trusting client-side validation alone
- Use `GlideRecordSecure` where user-context access control must be respected
- Call a Script Include from a Flow Designer custom action with defined inputs and outputs
- Build and use a single `GlobalErrorHandler` Script Include across every layer of an application

**Prerequisites:** Modules 0–5. The architecture patterns from Module 5 (layering, repository, service) are used throughout.

**Key APIs & concepts:** `sn_ws.RESTMessageV2`, HTTP methods/headers, OAuth/basic auth, timeouts and retries, ACLs, `GlideRecordSecure`, input validation, Flow Designer custom actions, `GlobalErrorHandler`.

| # | Topic | Type | Time | What it covers |
| --- | --- | --- | --- | --- |
| 6.1 | [REST API Integration — Part 1](module-6-integrations/00-rest-api-1.md) | 🎬 | 12 min | Introduces `RESTMessageV2` basics: endpoints, HTTP methods, headers, and parsing JSON responses. |
| 6.2 | [REST API Integration — Part 2](module-6-integrations/01-rest-api-2.md) | 🎬 | 12 min | Adds basic/OAuth authentication, timeouts and retries, and wraps the integration in a reusable Script Include. |
| 6.3 | [Secure Coding with Script Includes & GlideAjax](module-6-integrations/02-secure-coding.md) | 🎬 | 12 min | Covers ACLs and script protection, input validation, client-callable exposure risks, and `GlideRecordSecure`. |
| 6.4 | [Call a Script Include in a Flow Designer Custom Action](module-6-integrations/03-flow-designer-custom-action.md) | 🎬 | 10 min | Builds a custom action with defined inputs/outputs, a script step calling a Script Include, and returning data to the flow. |
| 6.5 | [How to Implement Global Error Handling](module-6-integrations/04-global-error-handling.md) | 🎬 | 11 min | Establishes try/catch patterns routed through a central error logger Script Include, and how errors surface to users and admins. |

**Common pitfalls**
- Calling `RESTMessageV2` directly from a Business Rule instead of isolating it behind a gateway class.
- Trusting client-side validation as if it were a security control.
- Logging errors with generic messages that give no layer, method, or record context — undebuggable a week later.

[Open module →](module-6-integrations/README.md)

---

## Module 7 · Final Exercise and Assessment

**Goal:** prove it. Build one production-quality application, score it, and plan the architect track.

This module stops teaching and starts testing. Every pattern from Modules 1–6 is required at once, in one application, with nowhere to hide a shortcut — which is exactly the point. The brief is **Weather-Aware Facilities Requests**: staff raise a facilities request such as "office too cold," and the business wants it automatically enriched with the current outside temperature for that location, an intake form that guides users intelligently, and high-impact requests prioritised automatically. You build this inside your `x_course_lab` scoped application, on a table extending Task, integrating with the free Open-Meteo weather API.

The required architecture is layered end to end: a **FacilitiesController** (client-callable Script Include extending `AbstractAjaxProcessor`) talks to the catalog item's client script over GlideAjax; it delegates to a **FacilitiesService**, which owns all business logic and priority rules and receives its two dependencies — a **WeatherGateway** (wrapping `RESTMessageV2` against Open-Meteo) and a **FacilitiesRepository** (using `GlideRecordSecure` for all reads and writes) — through constructor injection, exactly as taught in Module 5.2–5.3. Every layer routes its failures through a shared **GlobalErrorHandler**, taught in Module 6.5. The build runs in eight staged steps — data model, repository, gateway, service with DI, controller, UI, automation via an after-insert Business Rule of three or four lines, and cross-cutting error handling and ACLs — each with its own "done when" check, plus optional stretch goals such as moving enrichment to an asynchronous event, adding retry-with-backoff, and caching temperature lookups.

The **scoring rubric** grades the build out of 100 points across eight weighted criteria: functional completeness (15), layering & separation of concerns (20 — the single heaviest criterion, reflecting Module 5's centrality), dependency injection & testability (15), data access discipline (10), integration robustness (10), security (10), error handling & observability (10), and client-side quality & UX (10), plus up to 10 bonus points for stretch goals. Four performance levels — Exemplary, Proficient, Developing, Insufficient — describe each criterion concretely (for example, layering is Exemplary only if any single layer could be replaced without touching the others). Grade bands then translate the total score into a plain-language outcome: 90–100 is **Architect-ready**, 75–89 is **Job-ready developer**, 60–74 is **Developing**, and below 60 is **Not yet**, with each band pointing you either onward to the architect roadmap or back to the specific module behind your weakest criterion.

Questions on any topic in this module — or any module — go in the comments of that topic's YouTube video, where the instructor answers directly and other learners can see the answer. Verification of your finished build works differently: it is available to members of the **Exam Navigator** YouTube channel membership, who can submit their final exercise for review against the rubric and receive written, verified feedback rather than a self-score. You can join at [the Exam Navigator membership page](https://www.youtube.com/channel/UCMf7pje1x5iZkkEF-Y3PLSA/join).

**What you'll be able to do**
- Build a layered, production-quality scoped application end to end from a business brief
- Apply dependency injection, secure coding, external integration, and global error handling together in a single build
- Grade your own work honestly against a published 100-point professional rubric
- Explain and defend your architectural decisions the way you would in a design review or interview
- Identify your weakest rubric criterion and map it back to the specific module that teaches it
- Plan your certification path and progression towards the AI-ready ServiceNow Software Architect track

**Prerequisites:** All previous modules (0–6). Do not start the final exercise until the hands-on tasks in Modules 2–6 are complete — the exercise assumes every one of them.

**Key APIs & concepts:** scoped application (`x_course_lab`), table extending Task, `AbstractAjaxProcessor`, `GlideAjax`, constructor dependency injection, `sn_ws.RESTMessageV2`, Open-Meteo API, `GlideRecordSecure`, Repository pattern, `GlobalErrorHandler`, ACLs, after-insert Business Rule, Flow Designer custom action (stretch goal), `gs.eventQueue()` (stretch goal).

| # | Topic | Type | Time | What it covers |
| --- | --- | --- | --- | --- |
| 7.1 | [Final Exercise: Build a Production-Ready App](module-7-capstone/00-final-exercise.md) | 📖 | 4–6 hrs | Specifies the Weather-Aware Facilities Requests brief and the eight-stage layered build: data model, repository, gateway, DI-based service, controller, catalog UI, automation, and cross-cutting error handling. |
| 7.2 | [Scoring Rubric](module-7-capstone/01-scoring-rubric.md) | 📖 | 30 min | Grades the build on 100 points across 8 weighted criteria with four performance levels each, plus bonus points and grade bands. |
| 7.3 | [Submit Your Work for Verification](module-7-capstone/02-submit-for-verification.md) | 📖 | 15 min | Explains the two feedback routes — free YouTube comments for specific questions, and paid Exam Navigator membership for a reviewed, verified score. |
| 7.4 | [From Developer to ServiceNow Software Architect](module-7-capstone/03-architect-roadmap.md) | 📖 | 20 min | Lays out the four-stage path from beginner to AI-ready ServiceNow Software Architect and how each stage maps back to course modules. |
| 7.5 | [Certifications & Career Roadmap](module-7-capstone/04-certification-and-career.md) | 📖 | 30 min | Plans a certification sequence (starting with CSA and CAD), portfolio strategy, and job search approach. |

**Common pitfalls**
- Building the demo to "just work" rather than to the layering the rubric actually grades — an app with a rough UI and clean layers outscores a polished UI with logic buried in Business Rules.
- Skipping the fake-gateway substitution test, which is the only real proof that dependencies are genuinely injected rather than merely parameterised.
- Reading the rubric only after building, instead of before — which changes what you build, not just how you score it.

[Open module →](module-7-capstone/README.md)

---

## Suggested 7-week schedule

| Week | Modules | Focus | Weekly commitment |
| --- | --- | --- | --- |
| 1 | Module 0 + Module 1 | PDI live and configured; JavaScript essentials and the client/server execution model | ~1.5 hrs |
| 2 | Module 2 | A working catalog item with client-side validation using Client Scripts and UI Policies | ~1 hr |
| 3 | Module 3 | Full CRUD on a custom table from Scripts - Background, plus Business Rule automation | ~1.5 hrs |
| 4 | Module 4 | Logic moved out of duplicated scripts and into reusable Script Includes callable via GlideAjax | ~1 hr |
| 5 | Module 5 | A layered service with injected dependencies, plus event-driven and service-bus patterns | ~1.5 hrs |
| 6 | Module 6 | A live REST integration with secure coding practices and global error handling | ~1 hr |
| 7 | Module 7 | Final exercise built, self-scored against the rubric, and optionally submitted for verification | 5–7 hrs |

This pace assumes a few evenings a week and is only a suggestion — the course has no deadlines and no gatekeeping between modules. If you already have ServiceNow admin experience, Weeks 1–2 will likely take less time; if you are new to both ServiceNow and programming, give Weeks 1 and 3 more room, since Module 1's JavaScript foundations and Module 3's GlideRecord sequence carry the most new vocabulary per minute of any point in the course. Week 7 is intentionally the heaviest single week — the final exercise is scoped at 4–6 hours precisely because it is meant to feel like a real, if small, professional engagement rather than another topic-length exercise.

For guidance on how each page is laid out and how to work through the hands-on tasks on every topic, see [How to Use This Course](how-to-use.md). For how to ask questions in the YouTube comments and how to get your final build reviewed through Exam Navigator membership, see [Get Help & Get Verified](support-and-verification.md).
