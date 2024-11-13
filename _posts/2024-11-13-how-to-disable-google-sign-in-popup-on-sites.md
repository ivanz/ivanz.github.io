---
title: "How to disable the Sign in with Google pop-up across web sites"
author: Ivan Zlatev
layout: post
categories:
  - HowTo
---

I've gotten tired of all those "Sign in with Google" pop-ups, especially the bad/overly aggressive implementations that do it in every new tab branch opened of the same page (looking at you TripAdvisor), so here is how to remove it.

![bye bye](/content/2024-11-13-how-to-disable-google-sign-in-popup-on-sites/image-3.png)

## Steps for Web

1. Install your favourite ad blocker (e.g. I use [uBlock Origin](https://ublockorigin.com/)) Add the following custom rule and you are done

`||accounts.google.com/gsi/*`

## Steps for Android apps
1. **Go to `My Account` -> `Security` ([link](https://myaccount.google.com/security))**
2. **Scroll down to `Your connections to third-party apps and services` and click on `See all connections`**
  ![connections bit](/content/2024-11-13-how-to-disable-google-sign-in-popup-on-sites/image-2.png)
3. **Click on the gear/cog icon:**
    ![cog image](/content/2024-11-13-how-to-disable-google-sign-in-popup-on-sites/image-1.png)
4. **Turn off the `Sign in prompts` toggle and you're done ([direct link](https://myaccount.google.com/connections/settings))**
    ![toggle](/content/2024-11-13-how-to-disable-google-sign-in-popup-on-sites/image.png)