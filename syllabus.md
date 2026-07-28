# Course Syllabus

The complete curriculum. Eight modules, 37 topics. Each topic is one page with its video embedded, key concepts, and a hands-on task you complete in your own instance.

**Total instruction:** ~6 hours 45 minutes · **Final exercise:** 4–6 hours · **Suggested duration:** 7 weeks self-paced

## Curriculum at a glance

| Module | Title | Focus | Topics | Time |
| --- | --- | --- | --- | --- |
| [0](#module-0-getting-started-and-your-pdi) | Getting Started & Your PDI | Orientation, instance setup | 4 | ~55 min |
| [1](#module-1-javascript-and-scripting-foundations) | JavaScript & Scripting Foundations | JS subset, client vs server | 2 | ~40 min |
| [2](#module-2-client-side-scripting) | Client-Side Scripting | Forms, UI Policies, catalog | 4 | ~45 min |
| [3](#module-3-server-side-scripting) | Server-Side Scripting | GlideSystem, GlideRecord, Business Rules | 7 | ~1 hr 20 min |
| [4](#module-4-script-includes-and-glideajax) | Script Includes & GlideAjax | Reusable server code, client bridge | 4 | ~45 min |
| [5](#module-5-architecture-and-design-patterns) | Architecture & Design Patterns | OOP, DI, events, service bus | 6 | ~1 hr 15 min |
| [6](#module-6-integrations-security-and-reliability) | Integrations, Security & Reliability | REST, secure coding, error handling | 5 | ~1 hr |
| [7](#module-7-final-exercise-and-assessment) | Final Exercise & Assessment | Project, rubric, architect roadmap | 5 | ~5–7 hrs |

**Legend:** 🎬 video topic · 📖 written guide (video not yet recorded)

---

## Module 0 · Getting Started and Your PDI

**Goal:** understand the role, then get a working development environment **before writing any code**.
[Open module →](module-0-getting-started/README.md)

| # | Topic | Type | Time |
| --- | --- | --- | --- |
| 0.1 | [How to Become a ServiceNow Developer](module-0-getting-started/00-how-to-become-a-developer.md) | 🎬 | 12 min |
| 0.2 | [Meet Your Instructor & Kickstart Your Career](module-0-getting-started/01-meet-your-instructor.md) | 🎬 | 8 min |
| 0.3 | [Create Your Personal Developer Instance (PDI)](module-0-getting-started/02-create-your-pdi.md) | 🎬 | 15 min |
| 0.4 | [Configure Your PDI for Development](module-0-getting-started/03-configure-your-pdi.md) | 🎬 | 20 min |

## Module 1 · JavaScript and Scripting Foundations

**Goal:** write correct JavaScript and know exactly where each script executes.
[Open module →](module-1-foundations/README.md)

| # | Topic | Type | Time |
| --- | --- | --- | --- |
| 1.1 | [JavaScript Essentials for ServiceNow](module-1-foundations/00-javascript-essentials.md) | 📖 | 20 min |
| 1.2 | [Where Scripts Run: Client vs Server](module-1-foundations/01-where-scripts-run.md) | 📖 | 20 min |

## Module 2 · Client-Side Scripting

**Goal:** control form behaviour in the browser without hurting performance.
[Open module →](module-2-client-side/README.md)

| # | Topic | Type | Time |
| --- | --- | --- | --- |
| 2.1 | [Mastering Client Scripts for Beginners](module-2-client-side/00-client-scripts.md) | 🎬 | 14 min |
| 2.2 | [UI Policies Explained](module-2-client-side/01-ui-policies.md) | 🎬 | 12 min |
| 2.3 | [Catalog Client Scripts Explained](module-2-client-side/02-catalog-client-scripts.md) | 🎬 | 8 min |
| 2.4 | [Scripting in Catalog UI Policies](module-2-client-side/03-catalog-ui-policies.md) | 🎬 | 9 min |

## Module 3 · Server-Side Scripting

**Goal:** read and write platform data reliably, then automate it with Business Rules.
[Open module →](module-3-server-side/README.md)

| # | Topic | Type | Time |
| --- | --- | --- | --- |
| 3.1 | [Understanding GlideSystem for Efficient Scripting](module-3-server-side/00-glidesystem.md) | 🎬 | 11 min |
| 3.2 | [GlideRecord Practical Demo — Part 1](module-3-server-side/01-gliderecord-1.md) | 🎬 | 12 min |
| 3.3 | [GlideRecord Practical Demo — Part 2](module-3-server-side/02-gliderecord-2.md) | 🎬 | 12 min |
| 3.4 | [GlideRecord Practical Demo — Part 3](module-3-server-side/03-gliderecord-3.md) | 🎬 | 12 min |
| 3.5 | [GlideRecord Practical Demo — Part 4](module-3-server-side/04-gliderecord-4.md) | 🎬 | 12 min |
| 3.6 | [How Business Rules Execute (with Examples)](module-3-server-side/05-business-rules-execution.md) | 🎬 | 13 min |
| 3.7 | [Display Business Rules & g_scratchpad](module-3-server-side/06-display-business-rules.md) | 🎬 | 9 min |

{% hint style="info" %}
**Why this order changed:** you learn GlideRecord before Business Rules because Business Rule scripts are mostly GlideRecord code. Learning the API first makes the automation lesson obvious instead of overwhelming.
{% endhint %}

## Module 4 · Script Includes and GlideAjax

**Goal:** stop repeating yourself — package logic once and call it from anywhere.
[Open module →](module-4-script-includes-glideajax/README.md)

| # | Topic | Type | Time |
| --- | --- | --- | --- |
| 4.1 | [Script Includes Explained (Full Demo)](module-4-script-includes-glideajax/00-script-includes.md) | 🎬 | 13 min |
| 4.2 | [Script Include & GlideAjax](module-4-script-includes-glideajax/01-glideajax.md) | 🎬 | 12 min |
| 4.3 | [Script Include & Object.extendObject — Part 1](module-4-script-includes-glideajax/02-extendobject-1.md) | 🎬 | 10 min |
| 4.4 | [Script Include & Object.extendObject — Part 2](module-4-script-includes-glideajax/03-extendobject-2.md) | 🎬 | 10 min |

## Module 5 · Architecture and Design Patterns

**Goal:** design like an architect — separation of concerns, testability, and scale.
[Open module →](module-5-architecture/README.md)

| # | Topic | Type | Time |
| --- | --- | --- | --- |
| 5.1 | [Object-Oriented Principles for Architects](module-5-architecture/00-oop-principles.md) | 🎬 | 14 min |
| 5.2 | [How to Implement Dependency Injection — Part 1](module-5-architecture/01-dependency-injection-1.md) | 🎬 | 12 min |
| 5.3 | [Dependency Injection — Part 2](module-5-architecture/02-dependency-injection-2.md) | 🎬 | 12 min |
| 5.4 | [Script Include & GlideAjax for Architects](module-5-architecture/03-script-include-glideajax-arch.md) | 🎬 | 13 min |
| 5.5 | [Queueing & Event-Driven Architecture](module-5-architecture/04-queueing-event-driven.md) | 🎬 | 13 min |
| 5.6 | [Service Bus Architecture for Reusable Integrations](module-5-architecture/05-service-bus.md) | 🎬 | 12 min |

## Module 6 · Integrations, Security and Reliability

**Goal:** talk to the outside world safely, and fail predictably when it breaks.
[Open module →](module-6-integrations/README.md)

| # | Topic | Type | Time |
| --- | --- | --- | --- |
| 6.1 | [REST API Integration — Part 1](module-6-integrations/00-rest-api-1.md) | 🎬 | 12 min |
| 6.2 | [REST API Integration — Part 2](module-6-integrations/01-rest-api-2.md) | 🎬 | 12 min |
| 6.3 | [Secure Coding with Script Includes & GlideAjax](module-6-integrations/02-secure-coding.md) | 🎬 | 12 min |
| 6.4 | [Call a Script Include in a Flow Designer Custom Action](module-6-integrations/03-flow-designer-custom-action.md) | 🎬 | 10 min |
| 6.5 | [How to Implement Global Error Handling](module-6-integrations/04-global-error-handling.md) | 🎬 | 11 min |

## Module 7 · Final Exercise and Assessment

**Goal:** prove it. Build one production-quality application, score it, and plan the architect track.
[Open module →](module-7-capstone/README.md)

| # | Topic | Type | Time |
| --- | --- | --- | --- |
| 7.1 | [Final Exercise: Build a Production-Ready App](module-7-capstone/00-final-exercise.md) | 📖 | 4–6 hrs |
| 7.2 | [Scoring Rubric](module-7-capstone/01-scoring-rubric.md) | 📖 | 30 min |
| 7.3 | [Submit Your Work for Verification](module-7-capstone/02-submit-for-verification.md) | 📖 | 15 min |
| 7.4 | [From Developer to ServiceNow Software Architect](module-7-capstone/03-architect-roadmap.md) | 📖 | 20 min |
| 7.5 | [Certifications & Career Roadmap](module-7-capstone/04-certification-and-career.md) | 📖 | 30 min |

---

## Suggested schedule

| Week | Modules | Milestone |
| --- | --- | --- |
| 1 | Module 0 + Module 1 | PDI live and configured; first script runs |
| 2 | Module 2 | A working catalog item with client-side validation |
| 3 | Module 3 | Full CRUD on a custom table from Scripts - Background |
| 4 | Module 4 | Logic moved out of Business Rules into Script Includes |
| 5 | Module 5 | A service layer with injected dependencies |
| 6 | Module 6 | A live REST integration with global error handling |
| 7 | Module 7 | Final exercise built, scored, and published |

## Assessment summary

- **Hands-on tasks** on every topic page — do them in your PDI.
- **Knowledge check** at the end of every module — answer from memory.
- **Final exercise** in Module 7, graded on the 100-point [scoring rubric](module-7-capstone/01-scoring-rubric.md).
- **Optional verification** for [Exam Navigator members](https://www.youtube.com/channel/UCMf7pje1x5iZkkEF-Y3PLSA/join).

## Getting unstuck

Ask your question in the comments of the video for that topic — see [Get Help & Get Verified](support-and-verification.md).

---

**Next →** [How to Use This Course](how-to-use.md)
