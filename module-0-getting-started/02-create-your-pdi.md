# Create Your Personal Developer Instance (PDI)

_Part of Module 0 · Getting Started & Your PDI · Getting Started & Your PDI · [ServiceNow Developer Course](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)_

**Estimated time:** 15 min

## What you'll learn

By the end of this topic you will have your own free, running ServiceNow instance to use for every exercise in this course.

## Watch

{% embed url="https://www.youtube.com/watch?v=LcR6mRGbrcI" %}

[Watch on YouTube](https://www.youtube.com/watch?v=LcR6mRGbrcI)

## Overview

{% hint style="warning" %}
**Do this before you write any code.** Every hands-on task from Module 1 onwards assumes you have a working instance. Nothing later in this course works without it.
{% endhint %}

A **Personal Developer Instance (PDI)** is a full, private ServiceNow environment that ServiceNow gives developers **free of charge**. It is yours alone: you get admin rights, you can break it, and you can reset it. It is the single most important tool in this course.

## Key concepts

- **What a PDI is** — a personal, non-production ServiceNow instance for learning and building, with full admin access.
- **Free and self-service** — requested from the ServiceNow Developer Portal in a few minutes, no cost and no licence needed.
- **Hibernation** — an unused PDI goes to sleep after roughly 10 days of inactivity. You wake it from the Developer Portal in a minute or two.
- **Reclamation** — leave it asleep long enough and ServiceNow may take it back. You then simply request a new one, which is why you export your work.
- **Reset** — you can wipe a PDI back to a clean state if you make a mess. This is a feature, not a failure.
- **Release family** — you choose the release version. Always pick the newest available so you are learning current APIs.

## Step-by-step

### 1. Create your developer account

1. Go to the [ServiceNow Developer Portal](https://developer.servicenow.com).
2. Click **Sign up / Register** and create a free account.
3. Verify your email address and sign in.

### 2. Request your instance

1. From the top navigation, open **Manage → Instance** (also shown as **Request Instance**).
2. Choose the **latest release** when prompted.
3. Confirm the request and wait for provisioning — usually a couple of minutes.

### 3. Save your credentials immediately

When provisioning finishes you are given three things:

| Item | Example | Why you need it |
| --- | --- | --- |
| Instance URL | `https://dev123456.service-now.com` | Where you log in |
| Admin username | `admin` | Full platform access |
| Admin password | auto-generated | You cannot guess it later |

{% hint style="danger" %}
Copy all three into your notes now. You can reset the password from the Developer Portal, but saving them takes five seconds and saves you a detour later.
{% endhint %}

### 4. Log in and confirm it works

1. Open your instance URL in a browser and log in as the admin user.
2. Confirm you land on the platform home page.
3. In the navigation filter, type **All** and search for `Incident` — you should see the Incident module. Your instance ships with sample data.

### 5. Know how to wake it

If you come back after a break and the URL shows a hibernation page:

1. Return to the [Developer Portal](https://developer.servicenow.com).
2. Open **Manage → Instance**.
3. Click **Wake Instance** and wait one to two minutes.

## Hands-on

{% hint style="success" %}
Complete all of these before moving on.
{% endhint %}

- [ ] Create your free account at [developer.servicenow.com](https://developer.servicenow.com)
- [ ] Request a PDI on the latest release
- [ ] Save the instance URL, username, and password to your notes
- [ ] Log in successfully as admin
- [ ] Find the Incident list and confirm sample data is present
- [ ] Locate the **Wake Instance** button so you know where it is when you need it

## Troubleshooting

| Problem | Fix |
| --- | --- |
| No instances available | ServiceNow occasionally caps capacity. Wait an hour and request again. |
| Instance hibernated | Developer Portal → **Manage → Instance → Wake Instance**. |
| Lost the password | Developer Portal → instance page → reset the admin password. |
| Instance reclaimed | Request a new one, then re-import your exported work. |

## Resources

- [ServiceNow Developer Portal](https://developer.servicenow.com)
- [Personal Developer Instance guide](https://developer.servicenow.com/dev.do#!/guides/washingtondc/now-platform/pdi-guide/personal-developer-instance-guide-introduction)
- [Full course playlist](https://www.youtube.com/playlist?list=PLWMzEPW90q1Z9-po9BsvC_rHDf5mtubdg)
- Stuck? [Ask in the YouTube comments](../support-and-verification.md)

---
<!--NAV-->
[← Meet Your Instructor & Kickstart Your Career](01-meet-your-instructor.md) · [Configure Your PDI for Development →](03-configure-your-pdi.md)
