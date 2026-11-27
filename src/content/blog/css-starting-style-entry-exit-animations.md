---
title: "CSS @starting-style: Smooth Entry and Exit Animations Without JavaScript"
description: "How the CSS @starting-style rule and transition-behavior property let you animate elements entering and leaving the page with zero JavaScript."
author: "Web Design Paphos"
date: "2026-11-27"
category: "Web Design"
readTime: "8 min read"
---

When a visitor clicks a button to open a dropdown menu, a notification slides in from the corner, or a dialog fades into view, those small moments of motion communicate that something is happening. They guide attention, reduce cognitive load, and make interfaces feel polished and intentional rather than abrupt.

For years, achieving smooth entry and exit animations required JavaScript to add and remove classes, or to delay the removal of an element from the DOM until an animation could complete. That approach works, but it adds complexity and can introduce timing bugs.

In 2024, two related CSS features reached wide browser support: `@starting-style` and `transition-behavior: allow-discrete`. Together they let you define entry and exit animations entirely in CSS, with no JavaScript required. This guide explains how they work and where to use them on a real business website.

## The Core Problem: Animating Discrete Changes

CSS transitions work by interpolating between two values over time. If you change `opacity` from `0` to `1`, the browser smoothly animates every step in between.

The problem arises with what CSS calls "discrete" properties: properties that can only be in one of a fixed set of states, with no meaningful in-between value. The most important of these is `display`. An element is either `display: block` or `display: none`. There is no halfway point.

When you show or hide an element by changing its `display` property, the transition system cannot interpolate between the two states. The change happens instantly, which is why elements often appear or disappear with a jarring snap even when you have transitions defined on other properties like `opacity` or `transform`.

This is the core problem that `@starting-style` and `transition-behavior: allow-discrete` were designed to solve.

## What @starting-style Does

The `@starting-style` rule lets you define the initial style an element should start from when it first appears on the page, either by being added to the DOM or by transitioning from `display: none` to a visible state.

Without `@starting-style`, when you show a hidden element, the browser sets its style to its final rendered state immediately because there is no "before" state to transition from. By defining a starting style, you give the browser that before state, and it can transition smoothly from those values to the element's final values.

Here is a basic example. Suppose you have a notification bar that should fade in when shown:

```css
.notification {
  opacity: 1;
  transform: translateY(0);
  transition: opacity 0.3s ease, transform 0.3s ease;
}

@starting-style {
  .notification {
    opacity: 0;
    transform: translateY(-8px);
  }
}
```

When `.notification` is added to the DOM or transitions from hidden to visible, the browser now knows to start from `opacity: 0` and `transform: translateY(-8px)` and transition to its final values. No JavaScript class toggling is required.

## Animating Exit with transition-behavior

Entry animations are the easier half. Exit animations are where things get more interesting.

When you hide an element by setting `display: none`, the browser removes it from the layout instantly, before any transition can run. Your opacity or transform transition never plays because the element is already gone.

The `transition-behavior` property, with its value `allow-discrete`, changes this behaviour. It tells the browser to treat the change in a discrete property like `display` as part of the transition, holding the element in its current visible state for the duration of the transition before applying the final discrete change.

```css
.notification {
  opacity: 1;
  transform: translateY(0);
  transition:
    opacity 0.3s ease,
    transform 0.3s ease,
    display 0.3s allow-discrete;
}

.notification.hidden {
  display: none;
  opacity: 0;
  transform: translateY(-8px);
}

@starting-style {
  .notification {
    opacity: 0;
    transform: translateY(-8px);
  }
}
```

With this code, when you add the `.hidden` class, the browser:

1. Runs the opacity and transform transitions from their current values toward `0` and `translateY(-8px)`
2. Waits for all those transitions to complete
3. Only then applies `display: none`

The element fades and slides out before disappearing. This is the exact behaviour that previously required multiple JavaScript event listeners or a timer-based hack.

**One important detail on declaration order:** When using the `transition` shorthand, declare `transition-behavior: allow-discrete` on its own line after the shorthand. If you place it before, or bundle it in a way that gets overwritten by the shorthand, the effect will not work. The rule is to list it last.

## Popovers and Dialogs: Top-Layer Elements

Popovers and dialogs, created with the HTML `popover` attribute and the `<dialog>` element, render in what CSS calls the "top layer." This is a separate rendering layer that sits above everything else on the page. It is what lets dialogs and popovers appear above other content without needing `z-index` tricks.

For top-layer elements, you need one more property alongside `display`: the `overlay` property. Setting `overlay` to `allow-discrete` tells the browser to keep the element in the top layer during its closing transition. Without it, the element would leave the top layer the instant it is dismissed and would appear behind other page content while trying to fade out.

Here is the complete pattern for animating a popover:

```css
[popover] {
  opacity: 1;
  transform: scale(1);
  transition:
    opacity 0.2s ease,
    transform 0.2s ease,
    overlay 0.2s allow-discrete,
    display 0.2s allow-discrete;
}

[popover]:not(:popover-open) {
  opacity: 0;
  transform: scale(0.95);
}

@starting-style {
  [popover]:popover-open {
    opacity: 0;
    transform: scale(0.95);
  }
}
```

The `:popover-open` pseudo-class matches the popover while it is visible. The `@starting-style` block targets this state to define where the entry animation begins. The `:not(:popover-open)` block defines the exit state. The same pattern applies to `<dialog>` elements using `:open` or `:modal`.

## Animating the Backdrop

Popovers and dialogs include a `::backdrop` pseudo-element: the semi-transparent overlay that covers the page behind the popover. You can animate this independently using its own `@starting-style` rule.

```css
[popover]::backdrop {
  background-color: rgb(0 0 0 / 0.5);
  transition:
    background-color 0.2s ease,
    overlay 0.2s allow-discrete,
    display 0.2s allow-discrete;
}

[popover]:not(:popover-open)::backdrop {
  background-color: rgb(0 0 0 / 0);
}

@starting-style {
  [popover]:popover-open::backdrop {
    background-color: rgb(0 0 0 / 0);
  }
}
```

This fades the backdrop in when the popover opens and fades it out when the popover closes, all without a single line of JavaScript.

## Practical Use Cases on Business Websites

### Navigation Dropdowns

Multi-level navigation menus are one of the most common places where entry animations matter on a business site. A dropdown that appears instantly feels inexpensive. With `@starting-style`, the submenu can slide down and fade in as the user hovers over the parent item, creating a smoother and more professional experience.

The technique works well here because the submenu is often shown and hidden by toggling `display`, which previously made CSS-only animation impossible.

### Mobile Menu Overlays

A full-screen mobile menu sliding in from the side is a natural fit for these CSS features. The menu transitions from `display: none` to visible with a smooth slide and opacity fade, and closes with the same animation in reverse.

Before `allow-discrete`, developers would typically add a class that set `opacity: 0` and a transform, wait for the transition to finish using a `transitionend` event, and only then set `display: none`. That is six or more lines of JavaScript to do something that CSS can now handle on its own.

### Notification Toasts

Toast notifications that slide in from the corner and fade out after a few seconds are a staple of modern web UX. With `@starting-style` and `transition-behavior`, you can achieve this by simply adding and removing the toast element from the DOM, letting CSS handle the animation automatically.

This is especially useful when notifications are created dynamically in response to user actions, like submitting a form or completing a purchase.

### Tooltips and Hover Cards

Tooltips that appear on hover should do so with a subtle animation that communicates they belong to the element being hovered. A quick scale and fade entry, using `@starting-style`, achieves this in a few lines of CSS.

The same applies to hover cards: those small panels that appear when you hover over a user avatar or a product thumbnail. A clean entry animation makes the experience feel considered rather than bolted on.

### Form Validation Messages

Error messages and success confirmations that appear after form submission benefit from gentle entry animations. A message that fades in with a slight vertical slide draws attention to the feedback without being jarring. Because validation messages are often added to the DOM dynamically, `@starting-style` applies automatically without any extra JavaScript.

## Browser Support

Browser support for `@starting-style` is strong. The feature reached Baseline "Widely Available" status in late 2024, meaning it has been supported in Chrome, Edge, Firefox, and Safari for at least twelve months:

- **Chrome:** 117 and later
- **Edge:** 117 and later
- **Firefox:** 129 and later
- **Safari:** 17.5 and later

This covers well over 90% of browsers in active use globally as of 2026. For older browsers, the approach degrades gracefully: elements simply appear or disappear without an animation. The page functions correctly; it just lacks the motion. This is a sound progressive enhancement strategy for any business website.

If you need to detect support and provide a fallback, the CSS `@supports` rule makes this straightforward:

```css
@supports (transition-behavior: allow-discrete) {
  /* Animated entry and exit code */
}
```

You can also test from JavaScript using `CSS.supports('transition-behavior', 'allow-discrete')` and load a minimal animation library only for the small share of users on older browsers.

## Keeping Animations Accessible

Not every visitor wants motion on their screen. Some people experience vestibular disorders or sensitivity to motion that makes animations uncomfortable or disorienting. The `prefers-reduced-motion` media query lets you respect the system preference users set in their operating system.

```css
@media (prefers-reduced-motion: reduce) {
  .notification,
  [popover] {
    transition: none;
  }
}
```

With this in place, users who have enabled "Reduce Motion" in their device settings will see elements appear and disappear instantly. This is a simple two-line addition that covers an important accessibility consideration, and it is especially relevant for public-facing websites serving a wide range of visitors.

## Performance: Why CSS Animations Beat JavaScript

Aside from code simplicity, CSS transitions running on the compositor thread have a genuine performance advantage over JavaScript-driven animations. The compositor thread operates independently of the main JavaScript thread, which means your animations can stay smooth even when the browser is busy parsing scripts, handling network requests, or processing user input.

For businesses with visitors on older or lower-powered devices, the kind of hardware common across Cyprus and other markets where the latest flagship phones are not universal, this difference is noticeable. A CSS-animated modal that opens smoothly on a mid-range phone is a better user experience than a JavaScript-animated one that stutters while other scripts compete for the main thread.

Fewer JavaScript dependencies also mean smaller page bundles. Every utility function you can replace with native CSS is one less thing to download, parse, and execute before the page becomes interactive.

## Combining @starting-style with Other Modern CSS

`@starting-style` works naturally alongside other recent CSS capabilities. If you are using the native HTML `popover` attribute, it already handles keyboard focus, stacking, and light-dismiss behaviour. Adding `@starting-style` gives it the smooth animation that makes it feel complete.

The native `<dialog>` element pairs similarly well. Combined with anchor positioning, which lets you attach a tooltip or dropdown to its trigger element without JavaScript position calculations, you have a full set of tools for interactive UI components that require almost no JavaScript.

This direction in CSS is deliberate. Each feature builds on the others and reduces the surface area that JavaScript needs to cover. The result is websites that are lighter, faster, and easier to maintain.

## Summary

`@starting-style` lets you define the starting values for a CSS transition, so elements animate in when they first appear. `transition-behavior: allow-discrete` lets you animate discrete property changes like `display: none`, so elements can fade and slide out before actually disappearing. The `overlay` property with `allow-discrete` handles the same requirement for top-layer elements like popovers and dialogs.

Together, these three CSS features replace a category of JavaScript that web developers have been writing for over a decade. They work in all major browsers, degrade gracefully where unsupported, and deliver smoother animations with less code and better performance.

The next time you reach for a JavaScript animation library to handle a fade-in or slide-out effect, check whether `@starting-style` can do the job instead. In most cases, it can.

[Web Design Paphos](https://webdesignpaphos.github.io/) helps businesses in Cyprus build fast, modern websites.
