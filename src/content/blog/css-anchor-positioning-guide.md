---
title: "CSS Anchor Positioning: Build Tooltips and Popovers Without JavaScript"
description: "CSS Anchor Positioning reached Baseline in 2026. Learn how to build tooltips, dropdowns, and popovers using pure CSS with anchor-name and position-anchor."
author: "Web Design Paphos"
date: "2026-10-29"
category: "Web Design"
readTime: "8 min read"
---

For years, positioning a tooltip above a button felt deceptively simple, but delivering it reliably required either a JavaScript library, a pile of manual calculations, or both. You had to measure element positions, track scroll offsets, handle window resizes, and add logic to flip the tooltip when it was about to escape the viewport. Libraries like Floating UI and Popper.js exist specifically to handle this pain. In January 2026, that changed. CSS Anchor Positioning reached Baseline status, meaning it now works consistently across Chrome, Edge, Firefox, and Safari without a polyfill. This guide explains how it works, what you can build with it, and where JavaScript libraries still earn their place.

## What CSS Anchor Positioning Actually Solves

The fundamental problem is this: absolutely positioned elements are placed relative to their nearest positioned ancestor. If you want a tooltip to appear next to a button, you traditionally had two options.

The first was to make the button a positioned container, then put the tooltip inside it as a child. This works until you need the tooltip to escape a parent with `overflow: hidden`, or to appear above other elements in the stacking context.

The second was to use JavaScript to read the button's bounding rectangle, compute the tooltip's desired position, and write those coordinates to the tooltip's `style` attribute. This works, but the code runs after paint, which means a brief flash before the tooltip snaps to the right position. It also needs re-running on every scroll and resize.

CSS Anchor Positioning solves this by letting any element declare itself an anchor, and any other element, regardless of where it sits in the DOM, use that anchor as its positioning reference. The browser handles the geometry, including viewport flipping, using only CSS.

## The Core Properties

Three declarations are the foundation of every anchor layout.

### anchor-name

You apply `anchor-name` to the element that should serve as the reference point. The value is a dashed identifier, which looks like a CSS custom property name:

```css
.tooltip-trigger {
  anchor-name: --my-button;
}
```

The double-dash prefix is required. Multiple anchors on the same page each need a unique name.

### position-anchor

On the floating element, you declare which anchor it should attach to:

```css
.tooltip {
  position: absolute;
  position-anchor: --my-button;
}
```

The floating element must have `position: absolute` or `position: fixed`. Without one of those, the anchor connection exists but has no effect on layout, which can be a confusing silent failure.

### Placing the Float with position-area

Once the anchor is linked, you tell the browser where to place the floating element relative to that anchor. The `position-area` property divides the space around the anchor into a 3x3 grid, and you pick the cell:

```css
.tooltip {
  position: absolute;
  position-anchor: --my-button;
  position-area: top center;
}
```

This places the tooltip centred above the button. Other values include `bottom span-left`, `inline-start center`, `block-end`, and several more. You can also use the lower-level `anchor()` function inside inset properties for more precise control:

```css
.tooltip {
  position: absolute;
  position-anchor: --my-button;
  inset-block-end: anchor(top);
  left: anchor(center);
  transform: translateX(-50%);
}
```

This approach gives you explicit control: the tooltip's bottom edge sits at the button's top edge, and it is horizontally centred.

## Handling Viewport Overflow with @position-try

One of the most valuable parts of the spec is `@position-try`. This at-rule lets you define fallback positions that the browser applies automatically when the primary position would place the floating element outside the viewport.

```css
@position-try --below {
  position-area: bottom center;
}

@position-try --left {
  position-area: left center;
}

.tooltip {
  position: absolute;
  position-anchor: --my-button;
  position-area: top center;
  position-try-fallbacks: --below, --left;
}
```

The browser tries the positions in order: top first, then bottom, then left. It picks the first one that fits entirely within the viewport. This is the behaviour that previously required dozens of lines of JavaScript and a dependency on a geometry library.

### Specifying Try Order

From mid-2026, you can also use the shorthand `position-try-order: most-block-size` to tell the browser to prefer whichever fallback gives the floating element the most space, rather than simply picking the first one that fits. This is useful for dropdowns that display a scrollable list and need as much vertical room as possible.

## Building a Practical Tooltip

Here is a complete, minimal tooltip that works without any JavaScript:

```html
<button class="trigger">
  Hover me
  <span class="tooltip">This is a helpful tip</span>
</button>
```

```css
.trigger {
  anchor-name: --trigger;
  position: relative;
}

.tooltip {
  position: absolute;
  position-anchor: --trigger;
  position-area: top center;
  position-try-fallbacks: --below;
  margin-block-end: 8px;
  background: #1a1a1a;
  color: #fff;
  padding: 6px 12px;
  border-radius: 4px;
  font-size: 0.875rem;
  white-space: nowrap;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.15s;
}

@position-try --below {
  position-area: bottom center;
  margin-block-end: 0;
  margin-block-start: 8px;
}

.trigger:hover .tooltip,
.trigger:focus-visible .tooltip {
  opacity: 1;
}
```

This tooltip appears above the button, flips below if it would overflow the top of the viewport, fades in with a CSS transition, and remains hidden from pointer events when invisible. No JavaScript, no library, no layout recalculations on scroll.

## Combining with the Popover API

CSS Anchor Positioning pairs naturally with the HTML Popover API, which also became widely available in 2024 and 2025. The Popover API gives you accessible, dismissible overlays with the `popover` attribute and a dedicated `popovertarget` attribute on the trigger.

When you use a popover, the browser creates the anchor relationship automatically from the trigger to the popover element, so you may not need to write `anchor-name` at all. You only need to declare where the popover should appear relative to the trigger:

```html
<button popovertarget="my-menu">Open menu</button>
<div id="my-menu" popover>
  <ul>
    <li><a href="/about">About</a></li>
    <li><a href="/services">Services</a></li>
    <li><a href="/contact">Contact</a></li>
  </ul>
</div>
```

```css
#my-menu {
  position-area: bottom span-right;
  position-try-fallbacks: --above;
  margin: 0;
}

@position-try --above {
  position-area: top span-right;
}
```

The Popover API handles focus management, the Escape key, and closing the popover when you click outside. CSS Anchor Positioning handles placement. Together they replace a significant chunk of what custom dropdown code used to do.

## Browser Support and Progressive Enhancement

As of January 2026, CSS Anchor Positioning is Baseline Newly Available, supported in Chrome 125 and later, Edge 125 and later, Firefox 147 and later, and Safari 26 and later. Global browser usage figures suggest these versions collectively cover around 85 to 90 percent of active users, with older versions accounting for the rest.

### Writing a Graceful Fallback

Use `@supports` to provide a simpler alternative for browsers that do not support the feature:

```css
.tooltip {
  display: none;
}

@supports (anchor-name: --test) {
  .tooltip {
    display: block;
    position: absolute;
    position-anchor: --trigger;
    position-area: top center;
  }
}
```

For users on unsupported browsers, the tooltip simply does not appear. If the information is critical, put it inline rather than inside a tooltip. This is good accessibility practice regardless.

You can also check support at [caniuse.com](https://caniuse.com/css-anchor-positioning) to get a current breakdown by region. Traffic in Cyprus and nearby markets tends to skew toward newer mobile Safari and Chrome versions, so support rates are likely slightly above the global average.

## When JavaScript Libraries Still Make Sense

CSS Anchor Positioning is powerful, but it is not a replacement for every use case that Floating UI or Popper.js handle.

**Focus trapping inside dropdown menus.** When a dropdown opens, keyboard users expect Tab to cycle through the menu items without escaping to the page behind. CSS has no mechanism for this. The `inert` attribute helps, but wiring it correctly on open and close still requires JavaScript.

**Programmatic positioning logic.** If a floating element needs to respond to application state, such as always opening to the side with more items, a JavaScript layer remains the right place to compute and apply that decision.

**Virtualised lists.** If a dropdown contains hundreds of items rendered in a virtual scroll container, the positioning library typically needs access to scroll state and container boundaries. Pure CSS is not well-suited to this.

**Old browser requirements.** If your audience includes a significant share of users on older browsers and a degraded experience is not acceptable, a library with a JavaScript fallback is still the more reliable choice.

For the common cases, though, a tooltip above a button, a small popover with three links, an info badge that appears on focus, native CSS is now the right tool. It runs in the browser's layout engine, before JavaScript executes, which means no layout shift and no flash of incorrect positioning.

## Performance Benefits

Positioning-related JavaScript traditionally has performance implications. JavaScript that reads `getBoundingClientRect` or `offsetTop` forces the browser to recalculate layout before it can return accurate values, a pattern known as layout thrashing. In a scroll handler or a resize listener, this happens repeatedly.

CSS Anchor Positioning offloads that geometry entirely to the browser's layout engine, which already tracks element positions as part of its normal work. There is no additional read-layout-write cycle, no forced synchronous reflow, and no event listener overhead. On a device with a slow CPU, this difference can be noticeable, particularly on pages that have many positioned elements or that animate heavily.

## Inspecting Anchor Layouts in DevTools

Chrome DevTools supports anchor positioning inspection as of version 128. When you select a floating element, the Styles panel shows the resolved `position-area` value, and the Layout panel draws the anchor grid overlay on the canvas. This makes it straightforward to debug placement issues without reaching for `console.log`. Firefox DevTools added similar support around the same time as the Baseline announcement.

## Getting Started Today

If your codebase still uses Floating UI or Popper.js for simple tooltips or labels, CSS Anchor Positioning is worth adopting now. Start with the simplest cases: informational tooltips, small popovers, badge labels. Once you have the pattern in muscle memory, the code is faster to write, easier to maintain, and delivers a measurably lighter page.

A practical starting point is the MDN reference at [developer.mozilla.org/en-US/docs/Web/CSS/anchor-name](https://developer.mozilla.org/en-US/docs/Web/CSS/anchor-name), which stays current with the spec and includes browser compat tables. The CSS-Tricks guide also provides a thorough walkthrough of the `anchor()` function and the full `position-try` syntax.

The web platform is gradually absorbing many of the patterns that made JavaScript so necessary in the first place. Smooth scroll snapping, view transitions between pages, native modal dialogs, scroll-driven animations, and now precise tethered positioning: each of these is one fewer JavaScript dependency, one fewer event listener, and one fewer source of layout jank. Knowing when to use the new primitives and when to reach for a library is increasingly the core skill for modern front-end development.

[Web Design Paphos](https://webdesignpaphos.github.io/) helps businesses in Cyprus build fast, modern websites.
