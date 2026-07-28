# Scoring Rubric

_Part of Module 7 · Final Exercise & Assessment · Final Exercise & Assessment · [ServiceNow Developer Course](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)_

**Estimated time:** 30 min to self-assess

## How this rubric works

Your [final exercise](00-final-exercise.md) is scored out of **100 points** across **8 criteria**. Each criterion has four performance levels. Score yourself honestly — the point is to find your gaps, not to award yourself a certificate.

| Level | Meaning |
| --- | --- |
| **Exemplary** | Full points. Architect-grade work you could defend in a design review. |
| **Proficient** | Job-ready. Minor issues a reviewer would note but not block. |
| **Developing** | It works, but the design would cause problems in production. |
| **Insufficient** | Missing, or fundamentally wrong. Revisit the module. |

## Weighting

| # | Criterion | Points | Taught in |
| --- | --- | --- | --- |
| 1 | Functional completeness | 15 | All modules |
| 2 | Layering & separation of concerns | 20 | [Module 5](../module-5-architecture/README.md) |
| 3 | Dependency injection & testability | 15 | [Module 5.2–5.3](../module-5-architecture/01-dependency-injection-1.md) |
| 4 | Data access discipline | 10 | [Module 3](../module-3-server-side/README.md) |
| 5 | Integration robustness | 10 | [Module 6.1–6.2](../module-6-integrations/00-rest-api-1.md) |
| 6 | Security | 10 | [Module 6.3](../module-6-integrations/02-secure-coding.md) |
| 7 | Error handling & observability | 10 | [Module 6.5](../module-6-integrations/04-global-error-handling.md) |
| 8 | Client-side quality & UX | 10 | [Module 2](../module-2-client-side/README.md) |
| — | **Total** | **100** | |
| + | Stretch goals (bonus) | up to +10 | [Module 5.5](../module-5-architecture/04-queueing-event-driven.md), [6.4](../module-6-integrations/03-flow-designer-custom-action.md) |

---

## 1 · Functional completeness — 15 points

| Level | Points | Descriptor |
| --- | --- | --- |
| Exemplary | 14–15 | All eight stages work end to end. Submitting the catalog item produces a record with correct temperature and priority, with zero manual steps. |
| Proficient | 11–13 | All stages present; one minor behaviour is off (e.g. an edge-case priority rule). |
| Developing | 6–10 | Core flow works but one stage is missing or only partly wired up. |
| Insufficient | 0–5 | Multiple stages missing; the end-to-end flow does not complete. |

## 2 · Layering & separation of concerns — 20 points

| Level | Points | Descriptor |
| --- | --- | --- |
| Exemplary | 18–20 | Controller, service, gateway, and repository each have a single responsibility. The Business Rule is 3–4 lines of orchestration. No business logic outside the service. Any layer could be replaced without touching the others. |
| Proficient | 14–17 | Layers exist and are broadly respected; one small leak (e.g. a GlideRecord query in the service). |
| Developing | 8–13 | Some Script Includes exist, but responsibilities overlap and logic is spread between Business Rules and Script Includes. |
| Insufficient | 0–7 | Logic lives in Business Rules and client scripts. Script Includes are a dumping ground or absent. |

{% hint style="info" %}
**The single fastest check:** open your Business Rule. If it contains an `if` statement about business meaning, you have lost points here.
{% endhint %}

## 3 · Dependency injection & testability — 15 points

| Level | Points | Descriptor |
| --- | --- | --- |
| Exemplary | 14–15 | `FacilitiesService.initialize()` accepts repository and gateway, defaults to real implementations, and you can demonstrate the service running against a fake gateway that returns a fixed temperature. |
| Proficient | 11–13 | Dependencies are injectable, but defaults are hard-coded awkwardly or substitution was not demonstrated. |
| Developing | 6–10 | Dependencies are instantiated inside the methods that use them; substitution is impossible without editing code. |
| Insufficient | 0–5 | No separation of dependencies at all. |

## 4 · Data access discipline — 10 points

| Level | Points | Descriptor |
| --- | --- | --- |
| Exemplary | 9–10 | All GlideRecord access is confined to the repository. `getValue()`/`setValue()` used consistently. Queries are targeted with `setLimit()` / `addQuery()` — no unbounded table scans. |
| Proficient | 7–8 | Repository holds nearly all data access; one query elsewhere or one inefficient pattern. |
| Developing | 4–6 | GlideRecord calls scattered across layers, or queries with no conditions/limits. |
| Insufficient | 0–3 | Ad-hoc queries everywhere; no repository. |

## 5 · Integration robustness — 10 points

| Level | Points | Descriptor |
| --- | --- | --- |
| Exemplary | 9–10 | The gateway handles non-200 responses, timeouts, and malformed bodies with predictable return values. Breaking the API URL does not prevent the record from being created. |
| Proficient | 7–8 | Status codes handled; timeout or parse failure not explicitly covered. |
| Developing | 4–6 | Happy path only. An API failure throws or silently corrupts the record. |
| Insufficient | 0–3 | REST call inline outside a gateway, or absent. |

## 6 · Security — 10 points

| Level | Points | Descriptor |
| --- | --- | --- |
| Exemplary | 9–10 | GlideRecordSecure used for user-context reads/writes. All input is validated **server-side**, not just in the client script. Least-privilege ACLs on the table. No credentials or endpoints hard-coded in scripts. |
| Proficient | 7–8 | Server-side validation present; ACLs default rather than deliberately scoped. |
| Developing | 4–6 | Validation exists only in the client script; the server trusts whatever it receives. |
| Insufficient | 0–3 | No validation, no ACL consideration, or secrets in code. |

## 7 · Error handling & observability — 10 points

| Level | Points | Descriptor |
| --- | --- | --- |
| Exemplary | 9–10 | Every layer routes failures through `GlobalErrorHandler`. Each log entry names the layer, the method, the record sys_id, and the original message. A deliberate failure produces exactly one clear, actionable log entry. |
| Proficient | 7–8 | Central handler used in most places; log context is thin in one or two spots. |
| Developing | 4–6 | Scattered `try/catch` with `gs.error('error')`-style messages you could not debug from. |
| Insufficient | 0–3 | Errors swallowed silently or left to throw uncaught. |

## 8 · Client-side quality & UX — 10 points

| Level | Points | Descriptor |
| --- | --- | --- |
| Exemplary | 9–10 | Catalog client script uses asynchronous GlideAjax with a callback, gives clear user-facing guidance, and adds no perceptible form delay. Validation is duplicated server-side. |
| Proficient | 7–8 | Works asynchronously; messaging to the user is vague or inconsistent. |
| Developing | 4–6 | Synchronous server call, `alert()`-driven UX, or noticeable form lag. |
| Insufficient | 0–3 | No client-side validation, or it blocks the form. |

## Bonus — up to +10 points

| Stretch goal | Bonus |
| --- | --- |
| Enrichment moved to an asynchronous event / scheduled job | +3 |
| Retry with backoff on transient gateway failures | +2 |
| Temperature cached per location with a sensible TTL | +2 |
| Service invoked from a Flow Designer custom action | +2 |
| A test Script Include with fake dependencies and pass/fail logging | +3 |

Bonus points are capped at +10 and cannot take you above 100 for grading purposes — but they are exactly what a reviewer remembers.

---

## Grade bands

| Score | Band | What it means | What to do next |
| --- | --- | --- | --- |
| 90–100 | **Architect-ready** | Design-review-quality work. You are thinking in layers and failure modes. | Start the [architect roadmap](03-architect-roadmap.md). [Submit for verification](02-submit-for-verification.md) and use this as your portfolio centrepiece. |
| 75–89 | **Job-ready developer** | You would pass a technical interview and contribute on a real project. | Fix your two lowest criteria, then re-score. [Submit for verification](02-submit-for-verification.md). |
| 60–74 | **Developing** | It works, but the design would cause production pain. | Re-do the modules behind your lowest two criteria, then rebuild those stages. |
| Below 60 | **Not yet** | Fundamental gaps in layering or safety. | Do not move on. Revisit Modules 5 and 6 and rebuild from Stage 2. |

{% hint style="warning" %}
**Score the design, not the demo.** An app that works but buries logic in Business Rules scores lower than one with a rough UI and clean layers. That is deliberate — production systems are judged on what happens on the bad day, not the good one.
{% endhint %}

## Self-assessment sheet

Copy this into your notes and fill it in:

```
Criterion                              Score   / Max   Evidence / note
1  Functional completeness              ____    / 15    ______________________
2  Layering & separation of concerns    ____    / 20    ______________________
3  Dependency injection & testability   ____    / 15    ______________________
4  Data access discipline               ____    / 10    ______________________
5  Integration robustness               ____    / 10    ______________________
6  Security                             ____    / 10    ______________________
7  Error handling & observability        ____    / 10    ______________________
8  Client-side quality & UX             ____    / 10    ______________________
   Bonus (stretch goals)                ____    / +10   ______________________
                                        ------------
   TOTAL                                ____    / 100

Lowest two criteria: ______________________________
My fix plan:         ______________________________
```

## Resources

- [Final exercise brief](00-final-exercise.md)
- [Submit your work for verification](02-submit-for-verification.md)
- Stuck on a criterion? [Ask in the YouTube comments](../support-and-verification.md)

---
<!--NAV-->
[← Final Exercise: Build a Production-Ready App](00-final-exercise.md) · [Submit Your Work for Verification →](02-submit-for-verification.md)
