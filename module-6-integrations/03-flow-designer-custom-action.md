---
description: "Bridge low-code and pro-code by calling Script Includes from Flow Designer custom actions."
---

# Call a Script Include in a Flow Designer Custom Action

**Quick answer:** Bridge low-code and pro-code by calling Script Includes from Flow Designer custom actions. This lesson is a hands-on, step-by-step walkthrough you can follow in your own free ServiceNow Personal Developer Instance (PDI).

## Watch the video lesson

{% embed url="https://www.youtube.com/watch?v=ytJsCv6dzSM" %}

<!-- If the embed does not render, use this HTML block in GitBook: -->
<!--
<iframe width="560" height="315" src="https://www.youtube.com/embed/ytJsCv6dzSM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
-->

## What you will learn

By the end of this lesson you'll be able to bridge low-code and pro-code with custom actions.

## Overview

Bridge low-code and pro-code by calling Script Includes from Flow Designer custom actions.

> **Hands-on tip:** Follow along in your own PDI as you watch. Direct video link: https://www.youtube.com/watch?v=ytJsCv6dzSM

## Key concepts

- **Custom action inputs/outputs** — you declare typed input variables (e.g. a table reference or string) and output variables on the action, and those are the only values available inside the script step and the only values a flow can consume afterward.
- **Script step calling a Script Include** — inside the action's script step, instantiate your Script Include (e.g. `new MyUtility()`) and call a plain server-side method on it, keeping the actual logic in the Script Include so it stays testable and reusable outside of Flow Designer.
- **Returning data to the flow** — assign results to the action's output variables (for example `outputs.result = value;`) rather than using `return`, since Flow Designer reads the declared outputs object, not a function return value, to pass data to the next step in the flow.

## Hands-on

{% hint style="success" %}
Complete these in your Personal Developer Instance (PDI).
{% endhint %}

Build a custom action that wraps a utility Script Include and exposes its result to a flow.

- [ ] Create a simple Script Include (e.g. one method that takes a string and returns an uppercased version)
- [ ] Create a Flow Designer custom action with one string input and one string output
- [ ] In the action's script step, call your Script Include's method with the input variable
- [ ] Assign the returned value to the action's output variable
- [ ] Build a test flow that calls the custom action and logs the output with a Log step

**Done when:** running the test flow shows the transformed value in the flow's execution log, matching what the Script Include method would return when called directly in Scripts - Background.

## Frequently asked questions

### Why put logic in a Script Include instead of writing it directly in the script step?

Code inside a Script Include can be unit-tested from Scripts - Background, reused by other actions, business rules, or GlideAjax calls, and is easier to read in source control. The script step should stay thin — just map inputs to the Script Include call and map the result to outputs.

### Why doesn't my custom action return data with a normal return statement?

Flow Designer script steps don't use the function's return value; they read whatever you assigned onto the action's declared `outputs` object during execution. If an output variable is never assigned, it stays empty even if your script logic technically "returns" something internally.

### Can a custom action's script step call a client-callable Script Include?

It should call a normal server-side Script Include, not one built for GlideAjax. Custom actions execute entirely on the server, so there's no need for (and no benefit to) an `AbstractAjaxProcessor`-based include, which is designed for client-to-server calls.

## Discussion and questions

Have a question or want to share your progress? Post a comment under the video and the instructor will reply.

[Join the discussion on YouTube](https://www.youtube.com/watch?v=ytJsCv6dzSM)

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [ServiceNow Product Documentation](https://www.servicenow.com/docs/)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/channel/UCMf7pje1x5iZkkEF-Y3PLSA)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

## Continue the course

[← Secure Coding with Script Includes & GlideAjax](02-secure-coding.md) · [How to Implement Global Error Handling →](04-global-error-handling.md)

Back to: [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg) | [Course home](../README.md) | [Full syllabus](../syllabus.md)
