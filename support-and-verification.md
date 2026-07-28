# Get Help & Get Verified

You are not doing this alone. There are two channels: **ask questions publicly on YouTube**, and **get your work verified as an Exam Navigator member**.

## 1 · Ask your questions in the YouTube comments

Every topic page embeds its video. If you are stuck on that topic, scroll to the video and **post your question in the comments of that specific video**.

- Bill reads and answers the comments personally.
- Other learners see the answer, so your question helps people behind you.
- Because the question sits under the right video, the context is already clear.

{% hint style="info" %}
Post on the video for the topic you are stuck on — not on an unrelated video. That is how you get a fast, accurate answer.
{% endhint %}

### How to ask a question that gets a useful answer

Include these four things and you will usually get unstuck in one reply:

1. **Where you are** — module and topic name (e.g. "Module 4, GlideAjax").
2. **What you did** — the step you were on.
3. **What you expected vs what happened** — including the exact error text.
4. **Your code** — paste the relevant script, not a screenshot description of it.

**Good example:**

> Module 4 · GlideAjax. My client script calls `getAnswer()` but the callback returns null. Script Include is client-callable and extends AbstractAjaxProcessor. Code: `var ga = new GlideAjax('MyHelper'); ga.addParam('sysparm_name','getUserDept'); ga.getXMLAnswer(callback);` — Script Include method is `getUserDept: function() { return gs.getUser().getDepartmentID(); }`. What am I missing?

## 2 · Get your work verified — Exam Navigator membership

Doing the exercises is one thing. Knowing they are **right** is another. If you want your build reviewed instead of self-graded, join the channel as a member:

👉 **[Join Exam Navigator](https://www.youtube.com/channel/UCMf7pje1x5iZkkEF-Y3PLSA/join)**

As a member you can:

- Submit your **[final exercise](module-7-capstone/00-final-exercise.md)** for review against the official **[scoring rubric](module-7-capstone/01-scoring-rubric.md)**.
- Have your module hands-on work checked when you are not sure it is correct.
- Ask questions in member-only posts and get priority answers.
- Get feedback on architecture decisions — the part that separates a developer from an architect.

Full submission instructions are on [Submit Your Work for Verification](module-7-capstone/02-submit-for-verification.md).

{% hint style="success" %}
The course itself stays free and open source. Membership exists for the one thing content cannot do on its own: **verify your work and give you feedback.**
{% endhint %}

## 3 · Official documentation

Use these as your reference while you build:

- [ServiceNow Developer Portal](https://developer.servicenow.com) — PDI management, API reference, guides
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [ServiceNow Community](https://www.servicenow.com/community/) — broader Q&A beyond this course

## 4 · Improve this course

The content is open source. If a step is outdated or a link is broken, open an issue or a pull request on the GitHub repository behind this GitBook. Contributions are credited.

## 5 · Work with Bill directly

For mentorship, architecture advisory, or team training: [ilearntech.co.uk](https://ilearntech.co.uk).

---

**Next →** [Module 0 · Getting Started & Your PDI](module-0-getting-started/README.md)
