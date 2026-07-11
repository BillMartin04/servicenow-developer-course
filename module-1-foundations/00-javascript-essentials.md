# JavaScript Essentials for ServiceNow

_Part of Module 1 · JavaScript & Scripting Foundations · [ServiceNow Developer Course](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)_

## Watch

{% hint style="info" %}
**Video to record.** The written lesson below is complete and can be followed on its own.
{% endhint %}

## Overview

ServiceNow scripting *is* JavaScript. Before you touch a single Business Rule or Client Script, you need a solid grip on the JavaScript subset the platform uses. This lesson covers exactly that — no more, no less — with runnable examples you can paste into **Scripts - Background**.

{% hint style="warning" %}
**Two engines, two dialects.** Server-side scripts historically run on a Rhino engine with an **ES5** baseline (though newer releases support more modern syntax via a mode setting). Client scripts run in the user's **browser**, so modern JavaScript is available there. When in doubt server-side, write ES5-safe code: `var`, function declarations, no arrow functions or `let`/`const` unless you've confirmed support on your instance.
{% endhint %}

## Key concepts

### Variables and types

```javascript
var name = 'Bill';          // string
var count = 42;             // number
var active = true;          // boolean
var nothing = null;         // explicit "no value"
var notSet;                 // undefined
var user = { id: 1 };       // object
var roles = ['admin'];      // array (also an object)
```

**Truthiness** trips up beginners. These are all *falsy*: `false`, `0`, `''`, `null`, `undefined`, `NaN`. Everything else is truthy — including `'0'`, `'false'`, and `[]`.

```javascript
if ('') gs.info('never runs');
if ('false') gs.info('this DOES run — non-empty string is truthy');
```

### Functions and scope

```javascript
function greet(who) {
  return 'Hello, ' + who;
}
gs.info(greet('developer'));
```

`var` is **function-scoped**, not block-scoped. Closures let inner functions remember outer variables — the foundation of many ServiceNow patterns (and a common source of loop bugs).

### Objects and arrays

```javascript
var person = { first: 'Bill', last: 'Martin' };
person.role = 'Architect';          // add a property
gs.info(person.first + ' is ' + person.role);

var nums = [3, 1, 2];
nums.push(4);                       // [3,1,2,4]
nums.sort();                        // [1,2,3,4]
for (var i = 0; i < nums.length; i++) {
  gs.info(nums[i]);
}
```

### Iteration patterns

```javascript
// Object keys (ES5-safe)
var obj = { a: 1, b: 2 };
for (var key in obj) {
  if (obj.hasOwnProperty(key)) {
    gs.info(key + ' = ' + obj[key]);
  }
}
```

### Working with JSON

```javascript
var payload = { name: 'Incident', priority: 1 };
var text = JSON.stringify(payload);   // serialize to string
var back = JSON.parse(text);          // parse back to object
gs.info(text);
```

`JSON.stringify(obj)` is your best friend for logging objects during debugging.

## Common gotchas that break ServiceNow scripts

- **`==` vs `===`** — `0 == ''` is `true`; `0 === ''` is `false`. Prefer `===` to avoid surprises.
- **GlideRecord values are strings** — `gr.getValue('priority')` returns `'1'` (string), not `1`. Compare accordingly or use `parseInt()`.
- **Arrow functions / `let` server-side** — may not be supported depending on instance mode. Default to `var` and `function` on the server.
- **Mutating during iteration** — don't add/remove array items while looping over them.
- **`this` context** — inside callbacks and Script Includes, `this` may not be what you expect; capture it (`var self = this;`) when needed.

## Hands-on

{% hint style="success" %}
Run every snippet in **Scripts - Background** in your PDI and read the log output.
{% endhint %}

- [ ] Run each example above and confirm the logged output matches your expectation
- [ ] Write a function that takes an array of numbers and returns their sum
- [ ] Build an object, add two properties, and log it with `JSON.stringify()`
- [ ] Demonstrate truthiness: log whether `''`, `'0'`, `0`, and `[]` are truthy

## Resources

- [MDN JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
- [ServiceNow Scripting documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/@techtalkwithbill)
