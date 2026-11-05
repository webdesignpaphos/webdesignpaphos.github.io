---
title: "CSS Smooth Height Animations: Build Accordions Without JavaScript"
description: "Learn how CSS interpolate-size and calc-size() let you animate to height: auto natively, replacing JavaScript hacks with clean, fast pure CSS."
author: "Web Design Paphos"
date: "2026-11-05"
category: "Web Design"
readTime: "9 min read"
---

Animating an element's height has been one of the most annoying gaps in CSS for the better part of two decades. You could slide things open with a pixel value, but the moment a designer said "just expand to fit the content," developers reached for JavaScript. That workaround era is over. CSS now ships two native tools for this problem: `interpolate-size` and `calc-size()`. Together they let you animate to `height: auto`, `max-content`, `fit-content`, and other intrinsic sizing keywords directly in CSS -- no scripting required.

This guide explains what these properties do, how to use them, and shows practical patterns for the expandable UI components that appear on almost every business website.

## Why Height Animations Were Hard

CSS transitions and keyframe animations need concrete numeric values on both ends. You can animate `height` from `0px` to `300px` without issue. The transition has two numbers to interpolate between. The keyword `auto` does not work the same way. It instructs the browser to calculate the height from the content, and that value is not known until layout runs. CSS had no way to treat that calculated number as an animation endpoint.

The workarounds developers used were clever but clunky:

- **The max-height hack.** Set `max-height: 0` on the collapsed element and `max-height: 9999px` on the expanded state, with a `transition` on `max-height`. The element appears to open smoothly, but the closing animation is visibly slow or uneven because the browser is transitioning from an enormous value that far exceeds the content. Timing the ease curve to feel natural was guesswork.
- **JavaScript measurement.** Use `getBoundingClientRect()` or `scrollHeight` to read the full content height, set it as an inline `height` pixel value, trigger the transition, then remove the inline style on `transitionend`. This required multiple DOM reads, event listeners, and careful handling of edge cases like nested accordions or dynamic content. It worked, but it added JavaScript bundle weight to a purely visual effect.
- **CSS Grid row expansion.** Set `grid-template-rows: 0fr` and transition to `1fr`. The inner element needs `overflow: hidden`, but it avoids both the max-height timing problem and the JS measurement. It works in all modern browsers and is still a solid fallback.

All three are workarounds. CSS now has proper tools.

## What Is `interpolate-size`?

`interpolate-size` is an inherited CSS property that unlocks browser support for animating between lengths and intrinsic sizing keywords. It has two values:

- `numeric-only` (the default): the browser only interpolates between concrete lengths. Keywords like `auto` cannot be animated.
- `allow-keywords`: the browser can interpolate from a length to a sizing keyword and back.

The default is `numeric-only` because changing it globally would break existing stylesheets that rely on instant jumps when a keyword is involved. By opting in with `allow-keywords`, you tell the browser you want animations to work with those keywords.

The recommended way to enable it is once on `:root`:

```css
:root {
  interpolate-size: allow-keywords;
}
```

Because the property inherits, this single declaration applies to every element on the page. You can narrow the scope to a component if you prefer, but the global approach is simpler and matches the Chrome team's official guidance.

## Building a Pure CSS FAQ Accordion

FAQ sections are the most common use case for height animation on business websites. Here is a complete, accessible accordion pattern using `interpolate-size`:

### HTML structure

```html
<div class="accordion">
  <details class="accordion__item">
    <summary class="accordion__trigger">What areas do you cover?</summary>
    <div class="accordion__panel">
      <p>We work with clients across Cyprus, with a base in Paphos.</p>
    </div>
  </details>

  <details class="accordion__item">
    <summary class="accordion__trigger">How long does a project take?</summary>
    <div class="accordion__panel">
      <p>A typical website project takes four to eight weeks from kick-off to launch.</p>
    </div>
  </details>
</div>
```

Using the native `<details>` and `<summary>` elements is correct here: they provide built-in open/close behaviour, no JavaScript needed, and they are accessible to keyboard and screen reader users without additional ARIA attributes.

### CSS

```css
:root {
  interpolate-size: allow-keywords;
}

.accordion__panel {
  height: 0;
  overflow: hidden;
  transition: height 0.35s ease;
}

details[open] .accordion__panel {
  height: auto;
}
```

That is the entire implementation. When the user clicks the `<summary>`, the browser sets the `open` attribute on `<details>`, the CSS selector `details[open] .accordion__panel` activates, and `height` transitions from `0` to whatever height the content needs.

A few points worth noting:

- `overflow: hidden` is required. Without it, content is still visible during the close animation, spilling outside the element.
- The transition is on `.accordion__panel`, not on `<details>` directly. Transitioning the wrapper div gives you clean control over what animates.
- For browsers that do not yet support `interpolate-size: allow-keywords`, the accordion still works. It just opens and closes instantly without the slide. That is a perfectly acceptable experience for a minority of users, and it degrades gracefully with no broken layout.

### Styling the trigger

The default `<summary>` marker (the triangle) can be removed and replaced:

```css
.accordion__trigger {
  list-style: none;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 1.25rem;
  cursor: pointer;
  font-weight: 600;
}

.accordion__trigger::after {
  content: '+';
  font-size: 1.5rem;
  transition: transform 0.3s ease;
}

details[open] .accordion__trigger::after {
  transform: rotate(45deg);
}
```

The `+` rotates to form an `x` when open, giving clear visual feedback without any JavaScript event listeners.

## Using `::details-content` for Finer Control

Browsers are progressively shipping `::details-content`, a pseudo-element that lets you target the content area of a `<details>` element directly, without needing a wrapper div. Combined with `transition-behavior: allow-discrete`, this handles the display switching that happens when the element opens or closes.

```css
:root {
  interpolate-size: allow-keywords;
}

details::details-content {
  height: 0;
  overflow: hidden;
  transition:
    height 0.35s ease,
    content-visibility 0.35s allow-discrete;
}

details[open]::details-content {
  height: auto;
}
```

`transition-behavior: allow-discrete` is needed because `content-visibility` is a discrete property: it switches between visible and hidden states rather than interpolating. Without `allow-discrete`, the element would disappear immediately on close rather than fading out as the height collapses.

`::details-content` removes the wrapper div entirely, keeping the HTML cleaner. Check compatibility tables before relying on it in production, as support is newer than `interpolate-size`.

## Going Further with `calc-size()`

`interpolate-size` handles the common case of animating to `auto`. `calc-size()` does the same thing but also supports arithmetic on the calculated size. This is useful when you want the expanded panel to be slightly larger than its natural content, or when you need to combine a sizing keyword with a fixed length.

The syntax is:

```css
height: calc-size(auto, size + 2rem);
```

Here `size` represents the resolved value of `auto`. You can add, subtract, multiply, or divide:

```css
/* Open to natural height plus extra breathing room */
details[open] .accordion__panel {
  height: calc-size(auto, size + 1rem);
}

/* Open to half of max-content (useful for preview collapse) */
.card--collapsed {
  height: calc-size(max-content, size * 0.5);
}
```

One useful distinction: when you use `calc-size()` in a property value, the browser automatically enables `interpolate-size: allow-keywords` for that property. You do not need the global root declaration. This makes `calc-size()` a good scoped option when you only need the behaviour on a specific component.

## Other Intrinsic Sizing Keywords You Can Animate

`height: auto` is the most common use case, but `interpolate-size: allow-keywords` unlocks several others:

| Keyword | What it resolves to |
|---|---|
| `auto` | Height calculated from content and context |
| `max-content` | The smallest height where no content line wraps |
| `min-content` | The smallest height the content can occupy |
| `fit-content` | The available space up to `max-content` |

These open up more advanced patterns:

- **Animated width to fit-content:** Useful for tag chips or badge components that expand on hover to show a label.
- **Height to min-content:** Useful for shrinking a text block to its condensed form.
- **Progressive content reveal:** Animate to `max-content`, then clip to a shorter height on collapse, giving a true fold effect.

## Practical UX Use Cases for Business Websites

### Service detail cards

A services page can show a brief teaser for each service, then expand on click to show a detailed description. With `interpolate-size`, the card grows to fit the extra text without you needing to know how long that text is in advance.

This matters especially when content is managed through a CMS: the expanded height changes as editors update copy, and CSS handles it automatically.

### Pricing plan details

Pricing tables often need "What is included?" expand sections. Each plan might have a different number of line items, so a fixed max-height is imprecise. Animating to `auto` fits each plan correctly.

### Mobile navigation sub-menus

Nested menus on mobile commonly slide open to reveal child items. The parent item's height expands by the number of children, which varies per section. `interpolate-size` removes the need to calculate that height with JavaScript.

### Inline alerts and notifications

Alerts that appear after form submission or on validation errors can grow from zero to their natural content height, drawing attention smoothly without a jarring layout shift.

## Browser Support and Progressive Enhancement

As of late 2026, `interpolate-size: allow-keywords` has shipped in Chromium-based browsers, with Firefox support arriving in 2026 and Safari trailing slightly. The feature is flagged as Baseline on the Chrome developer documentation.

The correct approach is progressive enhancement:

```css
/* Works in all browsers: instant open/close, no animation */
.accordion__panel {
  height: 0;
  overflow: hidden;
}

details[open] .accordion__panel {
  height: auto;
}

/* Smooth animation in supporting browsers */
@supports (interpolate-size: allow-keywords) {
  :root {
    interpolate-size: allow-keywords;
  }

  .accordion__panel {
    transition: height 0.35s ease;
  }
}
```

Using `@supports` keeps the experience correct everywhere. Users on older browsers get functional accordions. Users on modern browsers get the smooth animation. No JavaScript is involved in either path.

## Performance Considerations

Animating `height` triggers layout (reflow), which is more expensive than animating `transform` or `opacity`. For most accordions and expandable sections, the layout cost is negligible: the component is small, and it only animates when the user interacts with it.

If you are running dozens of simultaneous height animations in a complex dashboard or running them on scroll, consider whether `transform: scaleY()` with `transform-origin: top` might serve the same visual effect at lower cost. For standard accordions and content toggles, `height` animation is fine.

## Why This Matters for Real Projects

The practical value of `interpolate-size` is not just about saving a few lines of JavaScript. JavaScript-driven height animations have always been fragile: they break when content loads asynchronously, when nested elements have their own transitions, when resize events fire mid-animation, or when a CMS editor adds an unexpected image to the panel. CSS handles all of these cases correctly because the browser recalculates layout before each animation frame.

Removing JavaScript from visual effects also improves page load time, reduces bundle size, and simplifies maintenance. Future contributors to the codebase do not need to find and understand the measurement logic. The CSS is self-documenting: open state has height auto, closed state has height zero.

For businesses running content-managed sites where editors update FAQ answers, service descriptions, and pricing details regularly, this reliability matters. The accordion will always open to exactly the right height, regardless of what the content is.

---

[Web Design Paphos](https://webdesignpaphos.github.io/) helps businesses in Cyprus build fast, modern websites.
