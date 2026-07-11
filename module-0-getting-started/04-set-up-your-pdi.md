# Set Up Your Personal Developer Instance (PDI)

_Part of Module 0 · Getting Started · [ServiceNow Developer Course](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)_

## Watch

{% hint style="info" %}
**Video to record.** Follow the written walkthrough below in the meantime — it is complete and self-contained.
{% endhint %}

## Overview

Every hands-on exercise in this course runs in your own free **Personal Developer Instance (PDI)** — a full ServiceNow environment that ServiceNow gives developers at no cost. This lesson gets you from zero to a working instance with a scratch application ready for the labs.

## Key concepts

- **What a PDI is** — a personal, non-production ServiceNow instance for learning and building. Yours alone, resettable, and free.
- **Instance lifecycle** — PDIs *hibernate* after ~10 days of inactivity. You wake them from the Developer Portal in a minute or two. They can also be *reset* to a clean state.
- **Release version** — you choose the release family (e.g. the latest named release). Pick the newest available so you learn current APIs.
- **Scoped vs Global** — you'll build labs in a dedicated scoped application to keep your work isolated and portable.

## Step-by-step

### 1. Create your account and request the PDI

1. Go to the [ServiceNow Developer Portal](https://developer.servicenow.com) and sign up (free).
2. From the top navigation, open **Manage → Instance** (or "Request Instance").
3. Choose the latest release when prompted and request the instance.
4. Wait for provisioning (usually a couple of minutes). You'll receive an **instance URL, admin username, and password** — save these.

### 2. Log in and orient yourself

1. Open your instance URL and log in as the admin user.
2. Confirm you can open the **All** menu (the navigation filter) and search for modules.
3. Open **System Definition → Scripts - Background** — this is where you'll run quick test scripts throughout the course.

### 3. Turn on developer tooling

1. In the navigation filter, type **System Diagnostics → Session Debug → Enable All** (or enable **JavaScript Log and Field Watcher**) so you can see client-side logs.
2. Open **System Logs → System Log → Script Log Statements** to view `gs.info()` output from server scripts.

### 4. Create your lab application

1. Open **Studio** (search "Studio" in the filter, or **All → Studio**).
2. Click **Create Application → Start from scratch**.
3. Name it `Course Lab`, scope name `x_course_lab`, and create it.
4. You'll build most exercises inside this scoped app so everything stays organised and exportable.

### 5. Verify with a first script

Open **Scripts - Background** and run:

```javascript
gs.info('PDI is working. Current user: ' + gs.getUserName());
var gr = new GlideRecord('incident');
gr.setLimit(1);
gr.query();
if (gr.next()) {
  gs.info('First incident number: ' + gr.getValue('number'));
}
```

Check **Script Log Statements** — you should see both messages. If you do, your environment is ready.

## Hands-on

{% hint style="success" %}
Complete these before moving on — every later module depends on them.
{% endhint %}

- [ ] Request your free PDI at [developer.servicenow.com](https://developer.servicenow.com)
- [ ] Log in and confirm you can open Studio and Scripts - Background
- [ ] Enable session/JavaScript debugging
- [ ] Create the `x_course_lab` scoped application
- [ ] Run the verification script and confirm the log output

## Troubleshooting

- **Instance hibernated?** Return to the Developer Portal and click **Wake Instance**.
- **Lost your password?** Reset it from the Developer Portal instance page.
- **Instance reclaimed?** If unused too long it may be released — just request a new one and rebuild your lab app.

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [PDI documentation & FAQ](https://developer.servicenow.com/dev.do#!/guides/washingtondc/now-platform/pdi-guide/personal-developer-instance-guide-introduction)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- [TechTalk with Bill on YouTube](https://www.youtube.com/@techtalkwithbill)

---
<!--NAV-->
[← Meet Your Instructor & Kickstart Your Career](01-meet-your-instructor.md) · [Next module: JavaScript & Scripting Foundations →](../module-1-foundations/README.md)
