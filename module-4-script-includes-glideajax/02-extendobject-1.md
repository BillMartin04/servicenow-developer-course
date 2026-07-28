---
description: "Learn how ServiceNow implements class inheritance with Object.extendsObject."
---

# Script Include & Object.extendObject — Part 1

**Quick answer:** Learn how ServiceNow implements class inheritance with Object.extendsObject. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=PXVt-CJc86M" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/PXVt-CJc86M" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to implement class inheritance in ServiceNow.

## Overview

Learn how ServiceNow implements class inheritance with Object.extendsObject.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=PXVt-CJc86M

## Key concepts

- **Prototype and extendsObject** — `Object.extendsObject(ParentClass, ChildClass)` wires `ChildClass.prototype` to inherit from `ParentClass.prototype`, so an instance of `ChildClass` can call parent methods it never redefined.
- **initialize() pattern** — `initialize()` is the constructor-equivalent method run when you `new` up a Script Include; a child class inheriting from a base class (or from `AbstractAjaxProcessor`) should generally rely on the parent's `initialize()` rather than defining its own, unless it explicitly calls the parent's version first.
- **Inheriting behaviour** — once extended, the child automatically gets every method on the parent's prototype for free, so shared logic (like common validation) can live once in the parent and be reused by every child class.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Create a base Script Include and a child that inherits its behaviour without redefining it.

- [ ] Create a Script Include `BaseGreeter` with `Class.create()`, an `initialize()` that sets `this.greeting = 'Hello'`, and a method `greet(name)` returning `this.greeting + ', ' + name`
- [ ] Create a second Script Include `FormalGreeter` and call `Object.extendsObject(BaseGreeter, FormalGreeter)` right after `Class.create()`
- [ ] Do NOT define an `initialize()` on `FormalGreeter` — let it inherit the parent's
- [ ] From Scripts - Background, run `new FormalGreeter().greet('Alex')` and log the result

**Done when:** the log shows `Hello, Alex`, proving `FormalGreeter` inherited both `initialize()` and `greet()` from `BaseGreeter` with zero code duplication.

## Frequently asked questions

### Where does `Object.extendsObject()` actually need to go in the script?

It goes immediately after `Class.create()` in the child Script Include, before the `prototype` definition: `var Child = Class.create(); Object.extendsObject(Parent, Child); Child.prototype = { ... };`. This wires the prototype chain before any methods are attached.

### If I extend a class, do I need to copy its initialize() into my child?

No, and you generally shouldn't. If the child defines no `initialize()`, it automatically uses the parent's. This matters most for client-callable Script Includes extending `AbstractAjaxProcessor` — defining your own `initialize()` there overwrites the parent's, which breaks `getParameter()` and the whole GlideAjax request.

### What happens to methods the child doesn't define itself?

JavaScript looks them up the prototype chain: if `ChildClass.prototype` doesn't have a method, it checks `ParentClass.prototype` next. That's why `Object.extendsObject` lets a child call any method the parent defines without rewriting it.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=PXVt-CJc86M)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/@techtalkwithbill)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← Script Include & GlideAjax](01-glideajax.md) · [Script Include & Object.extendObject — Part 2 →](03-extendobject-2.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
