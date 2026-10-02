---
title: "Progressive Web Apps: The Business Website Upgrade You Might Be Overlooking"
description: "PWAs combine the reach of a website with the feel of a mobile app. Here is what they are, why they work, and how to get one for your business."
author: "Web Design Paphos"
date: "2026-10-02"
category: "Web Design"
readTime: "8 min read"
---

Your website works. Visitors arrive, they browse, they leave. But a growing share of those visitors are on mobile, often on unreliable connections, and they expect an experience that feels closer to an app than a web page. Loading spinners, lost sessions when the signal drops, no way to get back quickly without searching again. These are friction points that quietly cost you customers.

Progressive Web Apps, or PWAs, are a practical solution to this problem. They are not a new technology in the sense of something experimental. Starbucks, Pinterest, Twitter, and thousands of smaller businesses have been running them for years. What has changed is that the tooling has matured to the point where any website can add PWA features without a full rebuild, and the business case is well documented with real numbers.

This guide explains what a PWA is, what results you can realistically expect, how the technology works under the hood, and what it takes to get one running on your own website.

## What Exactly Is a Progressive Web App?

A Progressive Web App is a regular website that has been enhanced with a specific set of browser capabilities. When those capabilities are switched on, the site can be installed to a device's home screen, cached so it loads instantly (and partially or fully offline), and in some cases send push notifications.

The key word is "progressive." The enhancements layer on top of a working website. Visitors who land on your site in a standard browser see nothing different. Visitors who use a modern browser on Android or iOS get the option to install the site like an app, and once installed, the experience changes significantly.

### How a PWA Differs from a Regular Website

A regular website fetches everything from the network on every visit. A PWA installs a service worker, a background script that sits between the browser and the network and can intercept requests. The service worker stores files in a local cache. When you return to the site, the service worker serves those files from the cache before the network responds, so the first render happens in milliseconds.

### How a PWA Differs from a Native App

A native iOS or Android app lives in the app store and requires a separate codebase, separate developer accounts, and app store review cycles. A PWA runs from your web server. One codebase covers every device. Updates go live the moment you deploy them, with no approval wait. And development costs are typically 60 to 75 percent lower than building a native app for two platforms.

The trade-off is that PWAs cannot access certain device hardware that native apps can, such as Bluetooth or NFC. For most business websites, those capabilities are irrelevant.

## Why PWAs Work: The Numbers

The most compelling argument for PWAs is what businesses have seen after deploying them.

**Starbucks** rebuilt its web ordering experience as a PWA. The result was a near-doubling of daily active web users and web orders that reached near-parity with their native mobile app orders. The PWA is 99.84 percent smaller than the native iOS app, which is 233 kilobytes compared to 148 megabytes. Customers in areas with slow mobile data could now actually use it.

**Pinterest** saw a 40 percent increase in time spent, a 44 percent increase in ad revenue from those users, and a 60 percent increase in core engagement metrics after their PWA launched. The new site loads in under five seconds on a 3G connection, down from 23 seconds for the previous version.

**Twitter Lite**, the PWA version of Twitter, achieved a 65 percent increase in pages per session and a 75 percent increase in tweets sent.

Across industries, studies consistently show PWAs achieving 36 percent higher conversion rates compared to the equivalent non-PWA experience. For a business website with even modest traffic, that difference compounds quickly.

## The Three Technical Pillars

Every PWA rests on three requirements. Understanding what they do helps you make informed decisions when working with a developer.

### HTTPS

Your site must be served over HTTPS. This is the baseline. Without it, browsers will not activate service workers or offer installation. If your site still runs on HTTP, or uses a mixed-content setup where some resources load over HTTP, that needs to be resolved before anything else.

Most hosting providers now enable HTTPS automatically through Let's Encrypt, which issues free SSL certificates. If your site is on HTTP, upgrading is usually a matter of minutes in your hosting control panel.

### The Web App Manifest

The Web App Manifest is a JSON file that tells the browser how your site should look and behave when installed as an app. It contains:

- **name** and **short_name**: the full name and the condensed label shown under the home screen icon
- **icons**: PNG files at multiple sizes, including 192x192 and 512x512 for Android, and a maskable version so the icon fills the shape used by the device (circle, squircle, or rounded square)
- **start_url**: the page that opens when the user taps the icon
- **display**: set to `standalone` for an app-like experience without browser chrome, or `browser` to keep the URL bar visible
- **theme_color**: the colour applied to the status bar and browser toolbar
- **background_color**: the background shown during the splash screen while the app loads

A missing icon at any of the required sizes, or an invalid `start_url`, will silently block the installation prompt. Chrome audits these requirements strictly.

### The Service Worker

The service worker is a JavaScript file registered by your website on the user's device. It runs in the background, separate from the main page, and intercepts all network requests made by your site.

Different resources call for different caching strategies:

**Cache first**: ideal for static assets like fonts, CSS, and JavaScript files that carry version hashes in their filenames. The service worker serves from cache instantly and only fetches from the network when the cache is empty.

**Network first**: appropriate for dynamic data like product prices or availability. The service worker tries the network, falls back to the cache if the request fails, so users see slightly stale data rather than an error.

**Stale while revalidate**: serves the cached version immediately for speed, then fetches a fresh version in the background for next time. Good for pages like blog posts or about pages that change occasionally but not constantly.

**Network only**: for anything that must be real-time, like checkout or authentication requests.

The most common mistake is using one strategy for everything. Mixing strategies per resource type is what makes a PWA feel genuinely fast.

## What Your Customers Actually Experience

Understanding the technical stack matters less than understanding what your customers will feel on the other end.

### The Install Prompt

On Android with Chrome, when a user visits your PWA and meets certain engagement thresholds (the browser detects you have visited the site more than once), a small banner appears at the bottom of the screen offering to install the site. On iOS in Safari, users tap the share icon and choose "Add to Home Screen." iOS 17 made this option more prominent when a valid manifest is present.

Once installed, the site gets its own icon on the home screen, an entry in the app drawer, and a splash screen on launch. The browser interface (address bar, back buttons) disappears, creating a full-screen experience.

### Offline and Intermittent Connectivity

A well-configured PWA means a user who loses their mobile signal mid-visit does not see the browser's error page. They see a custom offline page, or better still, the content they had already loaded, served from the cache.

For a business in Cyprus or anywhere with variable mobile coverage, this matters practically. A customer checking your opening hours or menu in an area with poor signal will either complete that task or bounce. The service worker determines which.

### Push Notifications

PWAs on Android support Web Push Notifications, which let you send updates to users who have installed your site. These behave identically to native app notifications: they appear in the notification tray even when the browser is closed.

iOS added Web Push support in iOS 16.4, which means iPhones and iPads can now receive push notifications from PWAs for the first time. This opens up the same use cases that businesses have relied on native apps for: appointment reminders, sale announcements, order status updates.

Push notifications require the user to grant permission. Asking at the right moment, not immediately on first visit but after the user has had a meaningful interaction, dramatically improves the accept rate.

## PWA Performance and Core Web Vitals

A PWA that installs but loads slowly has missed the point. Speed is not a feature; it is the foundation.

Chrome's Lighthouse tool (available in DevTools under the Audits tab) gives your site a PWA score alongside its performance score. A passing PWA audit requires:

- A valid manifest with the correct fields
- A registered service worker
- Pages that load over HTTPS
- A Largest Contentful Paint (LCP) of under 2.5 seconds
- Content that is usable without JavaScript (for resilience)
- A custom offline fallback page

Google's [PageSpeed Insights](https://pagespeed.web.dev/) runs the same Lighthouse checks on a remote server and gives you a real-world performance score alongside the lab score. Both matter. A site that scores 90 in the lab but 50 in the field has caching configured incorrectly or is over-relying on third-party scripts.

The practical performance targets for a PWA in 2026 are:

- **LCP under 2.5 seconds** on a 4G mobile connection
- **Time to Interactive under 3.8 seconds** on the same connection
- **Total page size for the shell (the minimum to display the interface) under 200 kilobytes**

The Workbox library, maintained by Google's Chrome team, provides pre-built service worker strategies that handle most caching logic without writing complex code from scratch. It ships with Webpack and Vite plugins that auto-generate service workers during your build process.

## Is a PWA Right for Your Business?

PWAs deliver the most value in specific scenarios. It helps to be clear about where you sit.

**High-value cases:**

- You have regular or returning customers who visit your site frequently (restaurants, service businesses, subscription services)
- A significant portion of your traffic is on mobile
- Your customers are in areas where mobile connectivity is inconsistent
- You want to send notifications about promotions, appointments, or orders without building a native app
- You are running an e-commerce store and want to reduce cart abandonment caused by slow load times

**Lower-value cases:**

- Your site is almost entirely informational and visited once (a landing page for a single product)
- The vast majority of your traffic is on desktop
- You are not set up to maintain and update a service worker over time

Even in lower-value cases, the baseline PWA setup, a manifest and a simple caching service worker, adds enough speed benefit to be worth the effort. The investment is small compared to the ongoing gain.

## Adding PWA Features to Your Existing Website

You do not need to start from scratch to become a PWA. Most websites can add the core features in a few hours of developer time.

### Step 1: Audit Your Current Site

Run Lighthouse in Chrome DevTools on your homepage. Switch to the PWA section of the report. It will list exactly what is missing: no manifest, no service worker, HTTP pages, missing icons. Each item links to documentation on how to fix it.

### Step 2: Create the Manifest

Create a file called `manifest.json` at the root of your site. Populate the required fields. Generate icons at 192x192 and 512x512 using a tool like [RealFaviconGenerator](https://realfavicongenerator.net/), which also generates a maskable icon variant. Link the manifest in your HTML `<head>`:

```html
<link rel="manifest" href="/manifest.json">
```

### Step 3: Register a Service Worker

Create a file called `sw.js` at your domain root. At its simplest, a precaching service worker using Workbox can be generated automatically during your build step. Manually, a minimal version caches the site shell on install and serves it on fetch:

```javascript
const CACHE_NAME = 'v1';
const SHELL = ['/', '/css/main.css', '/js/main.js', '/offline.html'];

self.addEventListener('install', e => {
  e.waitUntil(caches.open(CACHE_NAME).then(c => c.addAll(SHELL)));
});

self.addEventListener('fetch', e => {
  e.respondWith(
    caches.match(e.request).then(cached => cached || fetch(e.request))
      .catch(() => caches.match('/offline.html'))
  );
});
```

Register it from your main HTML or JavaScript:

```javascript
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/sw.js');
}
```

### Step 4: Create an Offline Page

Design a simple `/offline.html` page that matches your brand. It should tell the user they are offline, show your contact details (which are cached), and invite them to try again. This is far better than the browser's default error screen and keeps your brand in front of the user while they wait for connectivity to return.

### Step 5: Re-run Lighthouse

After the changes are deployed, run Lighthouse again. The PWA section should now show passing marks. Any remaining failures will point you to the specific issues.

## Tools Worth Knowing

**[Lighthouse](https://developer.chrome.com/docs/lighthouse/overview/)**: the official PWA auditing tool, built into Chrome DevTools and available as a CLI.

**[Workbox](https://developer.chrome.com/docs/workbox/)**: Google's service worker library. Handles cache strategies, precaching, and background sync. Reduces service worker code to a configuration file in most cases.

**[PWA Builder](https://www.pwabuilder.com/)**: Microsoft's free tool that generates a manifest and service worker from your existing URL, and packages the PWA for submission to the Microsoft Store, Google Play, and the Apple App Store.

**[RealFaviconGenerator](https://realfavicongenerator.net/)**: generates all required icon sizes, including maskable variants, from a single source image.

**[vite-plugin-pwa](https://vite-pwa-org.netlify.app/)**: if your site is built with Vite (which includes Astro, SvelteKit, and Nuxt), this plugin handles manifest generation and Workbox integration automatically.

## The Opportunity Cost of Waiting

Customers who install a PWA interact with a business's website three times more often than customers using the browser-only version. The install icon on the home screen acts as a daily reminder. The offline capability removes the friction of dropping out mid-task. The push notification channel is direct and permission-based.

These gains are not available to websites that are waiting for PWA support to become mainstream. It already is. The browsers support it, the frameworks support it, and the tooling makes implementation straightforward. The businesses that move first in a given market capture the engagement advantage before competitors do.

For a small business, the investment is a few hours of developer time and an ongoing commitment to keeping the manifest and service worker updated as the site evolves. The returns, higher engagement, lower bounce rates on mobile, and a direct notification channel, arrive immediately after deployment and compound over time.

---

[Web Design Paphos](https://webdesignpaphos.github.io/) helps businesses in Cyprus build fast, modern websites.
