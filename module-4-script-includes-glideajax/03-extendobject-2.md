---
description: "Part 2 continues the inheritance example with overriding and calling parent methods."
---

# Script Include & Object.extendObject — Part 2

**Quick answer:** Part 2 continues the inheritance example with overriding and calling parent methods. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=mDJEyReGg-U" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/mDJEyReGg-U" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to override methods and call the parent prototype.

## Overview

Part 2 continues the inheritance example with overriding and calling parent methods.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=mDJEyReGg-U

## Key concepts

- **Method overriding** — a child class can redefine a method with the same name as one on the parent's prototype; the child's version runs instead whenever that method is called on a child instance.
- **Calling the parent prototype** — inside an overriding method, `ParentClass.prototype.methodName.call(this, args)` explicitly invokes the parent's original implementation, letting the child add behaviour before or after it rather than replacing it entirely.
- **When to prefer composition** — if a "child" only needs a small piece of the parent's behaviour rather than an is-a relationship, calling another Script Include as a helper (composition) is often simpler and less fragile than building a deep inheritance chain.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Extend the greeter from Part 1, override its method, and still reuse the parent's logic.

- [ ] Reuse `BaseGreeter` from Part 1 (or recreate it) with `greet(name)` returning `this.greeting + ', ' + name`
- [ ] Create `LoudGreeter`, call `Object.extendsObject(BaseGreeter, LoudGreeter)`, and override `greet(name)` to call the parent version with `BaseGreeter.prototype.greet.call(this, name)` and return that result in uppercase
- [ ] From Scripts - Background, run `new LoudGreeter().greet('Alex')` and log the result
- [ ] Also log `new BaseGreeter().greet('Alex')` to compare the unmodified parent behaviour

**Done when:** `LoudGreeter` logs `HELLO, ALEX` while the untouched `BaseGreeter` still logs `Hello, Alex`, proving the override adds behaviour without duplicating the greeting logic.

## Frequently asked questions

### If I override a method, do I lose the parent's version entirely?

No, it's still reachable. Overriding only changes what runs when you call the method on a child instance; the parent's original implementation still lives on `ParentClass.prototype` and can be invoked explicitly with `ParentClass.prototype.methodName.call(this, ...)`.

### Why call the parent method instead of just rewriting the whole thing in the child?

Calling the parent keeps the shared logic in one place, so a future fix to the parent's method automatically applies to every child that calls it. Rewriting the whole method in the child duplicates logic and risks the two versions drifting apart over time.

### How do I decide between inheritance (extendsObject) and composition (calling another Script Include)?

Use inheritance when the child truly "is a" specialized version of the parent and needs most of its behaviour, like `LoudGreeter` being a kind of `BaseGreeter`. Use composition — just instantiating and calling another Script Include as a helper — when you only need one piece of unrelated functionality, since it keeps classes simpler and avoids deep, fragile inheritance chains.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=mDJEyReGg-U)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/channel/UCMf7pje1x5iZkkEF-Y3PLSA)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← Script Include & Object.extendObject — Part 1](02-extendobject-1.md) · [Object-Oriented Principles for Architects →](../module-5-architecture/00-oop-principles.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
