---
title: "CSS Scroll Snap: Create Polished Carousels and Showcases Without JavaScript"
description: "Learn how to use CSS Scroll Snap to build smooth, accessible carousels and scroll experiences that impress visitors without heavy JavaScript libraries."
author: "Web Design Paphos"
date: "2026-10-23"
category: "Web Design"
readTime: "9 min read"
---

If your website has a testimonial slider, a services carousel, or a portfolio gallery, there is a very good chance it is powered by a JavaScript library like Slick, Swiper, or Splide. These libraries work, but they add kilobytes to your page weight, require maintenance as dependencies update, and can introduce subtle accessibility problems if not configured carefully.

There is a native browser alternative that most designers and developers still overlook: CSS Scroll Snap. It has had solid browser support since 2019, works in Chrome, Edge, Firefox, Safari, and Samsung Internet, and lets you create smooth, polished scrolling experiences using only a handful of CSS properties. This guide walks through how it works, when to use it, and exactly how to implement it on a business website.

## What CSS Scroll Snap Actually Does

CSS Scroll Snap is a set of CSS properties that instruct the browser to lock scroll position to specific points after the user stops scrolling. Instead of a swipe or scroll landing awkwardly between two items, the browser snaps to the nearest defined point, producing the clean, paginated feel you see in native mobile apps.

The mechanism sits entirely in the browser's scroll engine. This means it inherits all the benefits of native scrolling: smooth physics, proper touch support, keyboard accessibility, and the browser's own optimised rendering path. Custom JavaScript carousels often override these, and that is where many carousel accessibility failures originate.

Browser support for CSS Scroll Snap is approximately 96.8% globally as of 2026. No polyfill is required for modern visitors.

## The Core Properties

CSS Scroll Snap uses two groups of properties: container properties and child properties.

### Container Properties

You apply these to the scrollable wrapper element.

**scroll-snap-type**

This is the main switch. It takes two values: an axis, and a strictness setting.

- Axis values: `x` (horizontal), `y` (vertical), `block`, `inline`, or `both`
- Strictness values: `mandatory` or `proximity`

```css
.carousel {
  overflow-x: auto;
  scroll-snap-type: x mandatory;
}
```

**scroll-padding**

This offsets where the snapped content sits within the container. Use it when you have a sticky header so the snapped section does not hide underneath it.

```css
.carousel {
  scroll-padding: 0 1rem;
}
```

**overscroll-behavior**

This stops the scroll from chaining to the parent page. Without it, once the user reaches the end of a horizontal carousel, their scroll gesture continues scrolling the page vertically, which feels broken.

```css
.carousel {
  overscroll-behavior-x: contain;
}
```

### Child Properties

You apply these to each individual item inside the scrollable container.

**scroll-snap-align**

This tells the browser which edge of each child to snap to: `start`, `center`, or `end`.

```css
.carousel-item {
  scroll-snap-align: start;
}
```

**scroll-margin**

Works like padding but from the child's perspective. Use it to push a specific item slightly away from the snap edge, for example to reveal a peek of the next item.

```css
.carousel-item {
  scroll-margin: 0 0.5rem;
}
```

## Mandatory vs Proximity: Choosing the Right Strictness

The choice between `mandatory` and `proximity` has real UX consequences. Getting it wrong can trap users or create jarring behaviour.

### When to Use mandatory

`mandatory` forces the browser to snap to a snap point after every scroll, no matter where the scroll stopped. Use it when:

- Every child item fits entirely within the container at any screen size
- You are building a full-page scroll where each section fills the viewport
- You are building a card carousel where cards have a fixed, predictable size

The critical rule: never use `mandatory` when a child element can be taller than the container. If a single card is taller than the visible area, the user will never be able to scroll to its lower content. The browser snaps away before they reach it.

### When to Use proximity

`proximity` snaps only when the scroll stops close enough to a snap point. If the user scrolls past the midpoint between two snap points, the browser snaps to the nearest one. If the scroll stops far from any snap point, it stays wherever it lands.

Use `proximity` for:

- Editorial or blog content where section lengths vary
- Any layout where content height is unpredictable
- Long pages where you want gentle snapping assistance without full enforcement

When uncertain, `proximity` is the safer default. It guides without trapping.

## Building a Horizontal Testimonial Carousel

Here is a complete, production-ready implementation you can adapt. It displays testimonial cards in a horizontal scroll with clean snapping, no JavaScript required for the scroll behaviour.

### HTML structure

```html
<section class="testimonials-wrapper">
  <div class="testimonials-track">
    <article class="testimonial-card">
      <blockquote>
        <p>"Working with this team transformed our bookings completely."</p>
        <footer>- Maria, Paphos Restaurant Owner</footer>
      </blockquote>
    </article>
    <!-- repeat for each testimonial -->
  </div>
</section>
```

### CSS

```css
.testimonials-wrapper {
  overflow: hidden;
  padding: 2rem 0;
}

.testimonials-track {
  display: flex;
  gap: 1.5rem;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  scroll-padding: 0 1.5rem;
  overscroll-behavior-x: contain;
  padding: 1rem 1.5rem;
  -webkit-overflow-scrolling: touch;

  /* Hide scrollbar visually but keep functionality */
  scrollbar-width: none;
}

.testimonials-track::-webkit-scrollbar {
  display: none;
}

.testimonial-card {
  flex: 0 0 clamp(280px, 80vw, 380px);
  scroll-snap-align: start;
  background: white;
  border-radius: 12px;
  padding: 2rem;
  box-shadow: 0 2px 16px rgba(0, 0, 0, 0.08);
}
```

The `flex: 0 0 clamp(280px, 80vw, 380px)` line is important. `flex-shrink: 0` prevents the cards from shrinking to fit the container, which would break the snap geometry. The `clamp()` function sizes cards responsively: they show nearly full-width on mobile and cap at 380px on larger screens.

## Building a Full-Page Vertical Scroll

Full-page vertical scroll sections create a presentation-style feel. Each section of content snaps into view as the user scrolls. This works well for landing pages, portfolios, and single-product sites.

```css
html {
  scroll-snap-type: y mandatory;
}

section {
  min-height: 100dvh;
  scroll-snap-align: start;
}
```

Using `100dvh` (dynamic viewport height) instead of `100vh` prevents the common mobile bug where the browser UI overlaps content on the first section.

For long sections that may overflow on smaller screens, switch to `proximity` on the `html` element and leave `mandatory` only on the shorter, hero-style sections.

## Adding Navigation Buttons Without a JavaScript Library

The scroll behaviour is handled by CSS, but prev/next buttons require a small amount of JavaScript to scroll to the correct position. Crucially, the JavaScript does not manage animation or timing. It just moves the scroll position and lets CSS Scroll Snap take over.

```javascript
const track = document.querySelector('.testimonials-track');
const cardWidth = track.querySelector('.testimonial-card').offsetWidth + 24; // 24 = gap

document.querySelector('.btn-next').addEventListener('click', () => {
  track.scrollBy({ left: cardWidth, behavior: 'smooth' });
});

document.querySelector('.btn-prev').addEventListener('click', () => {
  track.scrollBy({ left: -cardWidth, behavior: 'smooth' });
});
```

This is under 10 lines. Compare that to importing a 50kB carousel library and configuring its API.

For dot indicators that update as the active slide changes, the browser now fires `scrollsnapchange` events when snapping settles on a new target. This lets you highlight the correct dot without polling or intersection observers.

```javascript
track.addEventListener('scrollsnapchange', (event) => {
  const index = [...track.children].indexOf(event.snapTargetBlock || event.snapTargetInline);
  updateDots(index);
});
```

Note that `scrollsnapchange` is a relatively new addition. Check browser support and provide a fallback using an Intersection Observer if you need wider compatibility.

## Practical Use Cases on a Business Website

CSS Scroll Snap is not just for testimonials. Here are several high-value placements on a typical business website.

### Services or Product Showcase

Display your core services as scrollable cards. On desktop, two or three cards sit visible at once with a peek of the next, encouraging exploration. On mobile, a single card fills the screen. One CSS Scroll Snap container handles both without breakpoint-specific JS logic.

### Before/After Image Comparisons

A horizontal snap container with two full-width images creates a clean before/after gallery. Each swipe reveals exactly one image with no partial states.

### Team or Partners Section

Staff photos and bios are a natural fit for a horizontal card carousel. The snap behaviour ensures each bio is presented cleanly rather than cut off between two colleagues.

### Image Gallery

Instead of a lightbox library for a gallery, a scroll-snap grid lets users browse at their own pace. On touch devices this feels genuinely native.

### Onboarding or Feature Tour

Multi-step feature explanations, onboarding flows, or how-it-works sections scroll cleanly from step to step when each step is a snapped section.

## Accessibility Considerations

CSS Scroll Snap retains the browser's native scroll behaviour, which means keyboard navigation works automatically on most modern browsers. Arrow keys move between snap points in a focused scrollable container.

There are two things to add explicitly:

1. **Focus management for carousels.** When a user tabs into a carousel, focus should land on the active card. If a card scrolls into view on click of a nav button, move focus to that card too. This signals to screen readers that content has changed.

2. **Visible scroll affordance.** On desktop, a subtle peek of the next card signals that the content scrolls. On touch devices, momentum naturally suggests scrollability. If the design hides all scroll cues, add visual hints: a partial card, a gradient fade at the edge, or a drag hint animation.

Avoid hiding focus rings on carousel items. Keyboard users need to see where they are.

## Common Mistakes to Avoid

**Using mandatory on variable-height content.** This is the most frequent mistake. If any card in your carousel contains user-generated content of unpredictable length, use `proximity`.

**Forgetting flex-shrink: 0.** Without it, flex children shrink to fit the container and snap geometry breaks entirely.

**Omitting overscroll-behavior.** On touch devices, a horizontal carousel without `overscroll-behavior-x: contain` will accidentally trigger vertical page scrolling mid-swipe.

**Hiding the scrollbar on all platforms.** Hiding scrollbars reduces discoverability on desktop, where users may not think to click and drag. Consider showing a styled scrollbar on non-touch devices using a media query or feature query.

**Skipping scroll-padding with a sticky header.** If you have a fixed navigation bar, the top of each snapped section will hide behind it. `scroll-padding-top` offsets the snap position to compensate.

## Performance Benefits Compared to JavaScript Libraries

Loading a JavaScript carousel library typically adds between 30kB and 80kB to a page, depending on the library and whether you load a full build or tree-shake it. The library also executes JavaScript to calculate positions, manage touch events, and schedule animations.

CSS Scroll Snap offloads all of this to the browser's native scroll engine, which runs on the compositor thread. It is not blocked by the main JavaScript thread and does not contribute to Total Blocking Time, one of the Core Web Vitals metrics used in Google's ranking algorithm. For businesses focused on search performance, this is a meaningful practical advantage.

The CSS you need for a functional snap carousel is under 20 lines.

## A Note on Experimental Features

The CSS working group has been extending Scroll Snap with features like `::scroll-button` pseudo-elements and `::scroll-marker` groups, which would allow purely CSS-driven navigation buttons. These are experimental as of late 2026 and have partial browser support. Track the MDN compatibility tables before using them in production.

## Summary

CSS Scroll Snap is one of the most underused native browser features available to web designers today. It gives you smooth, app-quality scrolling experiences with a fraction of the code and weight of JavaScript libraries. For business websites, where page speed and accessibility both affect conversion, it is a practical upgrade worth making the next time you redesign a testimonial section, services row, or image gallery.

The core setup takes minutes to implement, the accessibility fundamentals are built in, and the performance benefits are measurable. Start with a horizontal testimonial carousel, use `mandatory` strictness with cards sized carefully at `flex: 0 0`, and add prev/next buttons with a dozen lines of JavaScript if you need them.

---

[Web Design Paphos](https://webdesignpaphos.github.io/) helps businesses in Cyprus build fast, modern websites.
