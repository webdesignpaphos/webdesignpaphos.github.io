---
title: "CSS View Transitions: Native Page Animations Without JavaScript Libraries"
description: "Learn how to add smooth, polished page animations to your website using the CSS View Transitions API, with no heavy libraries required."
author: "Web Design Paphos"
date: "2026-10-14"
category: "Web Design"
readTime: "7 min read"
---

For years, adding smooth page transitions to a website meant reaching for a JavaScript library. Barba.js, GSAP, Swup, Framer Motion -- these tools do the job, but each one adds kilobytes to your page, increases complexity, and creates another dependency to maintain.

The View Transitions API changes that equation. It is a native browser feature that lets you animate between two visual states of a page -- whether that is a route change inside a single-page app or a full navigation between two separate HTML pages. No library required. A few lines of CSS and a small amount of JavaScript are all it takes.

The API landed in Chromium-based browsers in 2023 and has matured significantly since. As of late 2026, Chrome, Edge, and Safari all support both same-document and cross-document transitions. Firefox support for cross-document transitions is still in progress, but the API degrades gracefully -- browsers that do not support it simply perform a normal instant navigation, so there is no risk of breaking anything.

## Why Page Transitions Matter for Business Websites

Before diving into the code, it is worth asking: does this actually matter for a real business website?

The short answer is yes, but not for the reasons you might expect.

The primary benefit is not the "wow factor" of a fancy animation. It is the sense of continuity and quality it creates for the visitor. When a user clicks a navigation link and the page change feels abrupt and jarring, there is a subtle signal that the website was built quickly. When the transition is smooth and intentional, the opposite impression forms.

Research from Google's UX team consistently shows that perceived performance matters as much as measured performance. A page that loads in 400ms but snaps in instantly can feel slower than a page that loads in 600ms but transitions gracefully. View Transitions make the interface feel like a cohesive product rather than a collection of separate documents.

For service businesses -- consultancies, architects, clinics, and agencies -- this polish adds credibility at exactly the moment a visitor is evaluating whether to get in touch.

## Two Types of View Transitions

The API handles two distinct scenarios, and understanding the difference shapes how you implement it.

### Same-Document Transitions

Same-document transitions apply when you update the DOM without navigating away from the current page. This is common in single-page apps (SPAs) built with frameworks like React, Vue, or Svelte, where the router swaps out sections of the UI without a full page load.

You trigger them in JavaScript like this:

```javascript
document.startViewTransition(() => {
  updateTheDom(); // your DOM change goes here
});
```

The browser captures a screenshot of the current state, runs your DOM update, then animates between the two states. By default it crossfades. The whole thing takes about four lines of code.

### Cross-Document Transitions

Cross-document transitions are more exciting for traditional multi-page websites, because they require no JavaScript at all. You add a single CSS rule to your stylesheet:

```css
@view-transition {
  navigation: auto;
}
```

That is it. Any page-to-page navigation on your site will now crossfade. Both the outgoing and incoming page need to include this rule, but that is straightforward if you use a shared stylesheet or layout component.

This means a static website built with Astro, Hugo, Jekyll, or plain HTML can have smooth page transitions without writing a single line of JavaScript.

## Customising the Default Crossfade

The default crossfade is clean but may not fit your design. The API exposes a set of pseudo-elements that let you style the transition with standard CSS animation properties.

### The Root Pseudo-Elements

When a transition runs, the browser creates a virtual overlay containing the old and new states:

- `::view-transition-old(root)` -- the screenshot of the page before navigation
- `::view-transition-new(root)` -- the incoming page

You can override the default animation on these like any CSS animation:

```css
@keyframes slide-from-right {
  from { transform: translateX(100%); }
}

@keyframes slide-to-left {
  to { transform: translateX(-30%); }
}

::view-transition-old(root) {
  animation: 300ms ease-in both slide-to-left;
}

::view-transition-new(root) {
  animation: 300ms ease-out both slide-from-right;
}
```

This produces a horizontal slide transition -- the current page exits left while the new page enters from the right. The effect is exactly what you see in native mobile apps, and it takes under ten lines of CSS.

### Element-Level Morphing with view-transition-name

The most visually impressive use of the API is shared element transitions -- animating a specific element from its position on one page to its position on another.

Imagine a service card on a listing page. When the user clicks it, the card image smoothly expands and repositions itself into the hero image on the service detail page. This is a complex effect that would normally require careful JavaScript orchestration. With the View Transitions API, you assign a matching name to the element on both pages:

```css
/* On the listing page */
.service-card img {
  view-transition-name: service-hero;
}

/* On the detail page */
.hero-image {
  view-transition-name: service-hero;
}
```

The browser matches the elements by name and automatically generates the morph animation between them. The key rule is that each `view-transition-name` value must be unique in the DOM at any given time -- you cannot assign the same name to two visible elements simultaneously.

For an agency or portfolio site, you could use this to morph a project thumbnail into the full-width hero image on the project page, creating a professional, app-like experience that immediately distinguishes the site from competitors.

### Dynamic view-transition-name for Lists

When you have a list of items -- a blog post grid, a services listing, a portfolio gallery -- you cannot hardcode a unique name for each one because you do not know how many items there will be. The solution is to assign the name dynamically in JavaScript or via inline styles:

```css
/* In a loop, each item gets its own name */
.card:nth-child(1) { view-transition-name: card-1; }
.card:nth-child(2) { view-transition-name: card-2; }
/* and so on */
```

In practice, this is often done with a JavaScript loop that reads an item's ID from a data attribute and assigns `element.style.viewTransitionName = 'item-' + id`. The result is that each card morphs individually rather than the entire page crossfading.

## Browser Support and Progressive Enhancement

As of October 2026, browser support is as follows:

- **Chrome and Edge:** full support for both same-document and cross-document transitions (since version 126 and later)
- **Safari:** full support since Safari 18.4, including cross-document transitions
- **Firefox:** same-document transitions are supported; cross-document transitions are still behind a flag

The global usage coverage for same-document transitions is above 80% of browsers worldwide. For cross-document transitions, it is lower due to Firefox, but still reaches the majority of users on most sites.

The good news is the API is designed for progressive enhancement. Browsers that do not support it simply navigate normally -- no broken layouts, no JavaScript errors. This means you can implement View Transitions today without worrying about compatibility.

Always wrap programmatic calls in a feature check:

```javascript
if (document.startViewTransition) {
  document.startViewTransition(() => updateTheDom());
} else {
  updateTheDom(); // instant fallback for unsupported browsers
}
```

## Accessibility: Respecting Motion Preferences

One of the most important rules when working with any animation is respecting the `prefers-reduced-motion` media query. Some users have vestibular disorders or motion sensitivity, and unexpected movement can cause genuine discomfort.

Always add a reduced-motion override:

```css
@media (prefers-reduced-motion: reduce) {
  ::view-transition-group(*),
  ::view-transition-image-pair(*),
  ::view-transition-old(*),
  ::view-transition-new(*) {
    animation: none !important;
  }
}
```

This turns off all transition animations for users who have opted out of motion. The navigation still works -- it just becomes an instant swap. WCAG 2.2 guidelines require that motion not be the sole mechanism for conveying information and that users can disable animations. This override satisfies both requirements and adds only four lines to your stylesheet.

## A Complete, Minimal Setup for a Multi-Page Site

Here is a full, production-ready implementation for a regular HTML/CSS website. This goes in a shared stylesheet included on every page:

```css
/* Opt every page into cross-document transitions */
@view-transition {
  navigation: auto;
}

/* Custom fade transition */
::view-transition-old(root) {
  animation: 250ms ease-in both fade-out;
}

::view-transition-new(root) {
  animation: 250ms ease-out both fade-in;
}

@keyframes fade-out {
  to { opacity: 0; }
}

@keyframes fade-in {
  from { opacity: 0; }
}

/* Always respect user motion preferences */
@media (prefers-reduced-motion: reduce) {
  ::view-transition-old(*),
  ::view-transition-new(*) {
    animation: none !important;
  }
}
```

That is roughly 25 lines of CSS. No JavaScript. No external dependencies. No npm packages to audit or update. Frameworks like Astro make it straightforward to include this in a shared layout component, applying it site-wide in under a minute.

## Performance Considerations

Because the browser handles the rendering internally, View Transitions are generally more performant than JavaScript-driven alternatives. The animation runs on the compositor thread, which means it avoids layout recalculations and stays smooth even on lower-powered devices.

A few things to keep in mind:

**Keep durations short.** Aim for 200ms to 350ms for most transitions. Faster feels abrupt; slower feels sluggish. The sweet spot is around 250ms to 300ms for a fade, slightly longer for a slide.

**Test on real devices.** What feels smooth on a development machine may stutter on a mid-range Android phone. Budget devices have significantly less GPU memory available for compositing, and heavy transitions can drop frames.

**Avoid transitioning complex trees.** The browser takes a screenshot of the entire page during the transition. On pages with hundreds of complex elements, this capture step can itself take time. Keep transitions focused on meaningful elements rather than applying them everywhere.

**Use Chrome DevTools.** The Animations panel lets you inspect and slow down the transition playback to catch timing issues. The Performance panel shows whether the animation is running on the compositor thread or causing layout recalculations.

## Replacing Libraries: The Maintenance Argument

The clearest practical argument for adopting the View Transitions API is the removal of third-party dependencies. Common libraries used for page transitions carry meaningful weight:

- Barba.js: approximately 7KB gzipped
- Swup: approximately 6KB gzipped
- Framer Motion (used only for page transitions): 30KB or more gzipped

Beyond file size, each library represents a maintenance commitment. Dependencies need to be updated when browsers change their behaviour, when security vulnerabilities are found, or when the framework you use updates its internal routing in a way that breaks third-party transition hooks.

Replacing these with a native browser feature saves real bytes, reduces maintenance overhead, and removes a potential attack surface. For websites where Core Web Vitals scores and Lighthouse performance ratings are priorities -- and for most business websites they should be -- that is a meaningful improvement that compounds over time.

## When to Use It and When Not To

View Transitions work best for top-level page or route changes. They feel natural and expected at the navigation level because they mirror the behaviour of native apps that users are already familiar with.

They are less appropriate for small, frequent UI updates -- toggling a dropdown, updating a counter, switching between tabs on the same page. These interactions have existing animation conventions, and adding a full view transition to them can feel heavy-handed.

Also consider your audience and brand. A law firm or financial services business serving clients across Cyprus would likely want a minimal fade transition -- clean and professional, reinforcing efficiency. A creative studio or portfolio site targeting international clients could lean into a more dramatic slide or morph effect. The API is flexible enough to accommodate the full range.

The element morphing feature via `view-transition-name` is worth deploying when you have a clear visual relationship between two pages -- a card that becomes a hero image, a list item that becomes a full detail view. In those situations, the morphing transition communicates the relationship between pages in a way that text navigation never could. In other situations, a simple fade is almost always the better choice.

## Getting Started

The best way to learn the View Transitions API is to add the single `@view-transition { navigation: auto; }` rule to an existing project and observe the difference. Then layer in the pseudo-element customisation and finally experiment with `view-transition-name` on individual elements.

MDN's "Using the View Transition API" guide is the most reliable reference, covering both modes with worked examples. For more advanced patterns including element morphing across list pages, the Chrome for Developers documentation from the Google Chrome team shows techniques that go well beyond the basics.

The API is a genuine step forward for the web platform. A feature that once required a dedicated library, careful event handling, and ongoing maintenance now lives in CSS and a single function call. For any site that cares about the quality of its user experience, adopting it is one of the higher-return improvements available today.

---

[Web Design Paphos](https://webdesignpaphos.github.io/) helps businesses in Cyprus build fast, modern websites.
