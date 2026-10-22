---
title: "CSS :has() Selector: The Parent Selector That Changes How You Write Styles"
description: "Learn how the CSS :has() selector lets you style parent elements, replace JavaScript logic, and write cleaner, smarter stylesheets."
author: "Web Design Paphos"
date: "2026-10-22"
category: "Web Design"
readTime: "8 min read"
---

For years, web developers had a wish: a way to style a parent element based on what it contains. CSS could select children, siblings, and descendants, but not parents. If you needed to change how a card looked when it had an image inside it, you reached for JavaScript. If you wanted to highlight a form field wrapper when its input was invalid, you wrote event listeners. That workaround era is over.

The CSS `:has()` pseudo-class -- sometimes called the "parent selector" -- is now supported in every modern browser. It solves one of the oldest problems in CSS and opens up a range of design patterns that previously required JavaScript. This guide walks through what it does, why it matters, and how to use it practically in real projects.

## What Is the CSS :has() Selector?

At its simplest, `:has()` lets you select an element based on whether it contains a matching descendant. The basic syntax looks like this:

```css
element:has(descendant-selector) {
  /* styles applied to the parent */
}
```

For example, to style a card differently when it contains an image:

```css
.card:has(img) {
  display: flex;
  gap: 1.5rem;
}
```

Without an image, the card renders as a standard block. With an image, it switches to a side-by-side flex layout. That conditional logic -- checking what is inside an element -- used to require JavaScript to add or remove a class. Now it is pure CSS.

## Browser Support in 2026

Support for `:has()` is now excellent. Chrome, Edge, Safari, and Firefox all ship it. Firefox was the last major browser to add support, reaching full compatibility from version 121 onwards. For practical purposes, if you are building for modern browsers -- which most business websites target -- you can use `:has()` without a fallback.

If you need to be cautious, CSS feature detection handles it cleanly:

```css
@supports selector(:has(img)) {
  .card:has(img) {
    display: flex;
  }
}
```

That `@supports` block applies the styles only in browsers that understand `:has()`. Browsers without support simply skip the block, leaving the default styles in place. This is progressive enhancement at its cleanest.

## Styling Parent Elements Based on Their Content

The most immediate use case is parent-child conditional styling. Consider a product card component that can appear with or without a product image. Rather than creating two separate classes or toggling classes from JavaScript, one CSS rule handles both states:

```css
.product-card {
  padding: 1.5rem;
  border-radius: 8px;
  background: white;
}

.product-card:has(.product-image) {
  display: grid;
  grid-template-columns: 200px 1fr;
  gap: 2rem;
  align-items: start;
}
```

Text-only cards stack vertically. Cards with images switch to a two-column grid. The HTML structure stays the same. The styling adapts automatically to the content.

This pattern is valuable in content management systems where editors add or remove images from posts, and the layout should respond without any developer involvement.

## Form Validation Without JavaScript

One of the most practical applications of `:has()` is form validation styling. HTML5 provides native input validation through attributes like `required`, `pattern`, and `type="email"`. Browsers expose validation state through pseudo-classes like `:valid`, `:invalid`, and `:user-invalid`. With `:has()`, you can propagate that state up to the parent container and style the entire field group accordingly.

Here is a pattern for a form field wrapper:

```css
.field-group {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.field-group:has(input:user-invalid) {
  background: #fff5f5;
  border-left: 3px solid #e53e3e;
  padding-left: 1rem;
}

.field-group:has(input:user-invalid) label {
  color: #e53e3e;
}

.field-group:has(input:focus) {
  background: #f0f7ff;
  border-left: 3px solid #3b82f6;
}
```

The `:user-invalid` pseudo-class is important here. Unlike `:invalid`, which fires immediately when the page loads on required fields, `:user-invalid` only activates after the user has interacted with the field. This avoids showing error states before a user has had a chance to type anything.

This approach gives you cohesive validation feedback -- the label, the wrapper, and the input all respond to the same state -- without a single line of JavaScript.

## Navigation: Highlighting Active Menu Items

Navigation menus often need a visual indicator on the parent list item that contains the active page link. With standard CSS, you could style the active link itself, but not the surrounding list item. With `:has()`, this is trivial:

```css
nav ul li {
  padding: 0.5rem 1rem;
  border-radius: 6px;
}

nav ul li:has(a.active) {
  background: rgba(59, 130, 246, 0.1);
  font-weight: 600;
}

nav ul li:has(a:hover) {
  background: rgba(59, 130, 246, 0.05);
}
```

Now the entire list item -- not just the link -- responds to hover and active states. This gives you more control over padding, background, and borders without restructuring your HTML or adding wrapper divs.

## Quantity Queries: Responsive Layouts Based on Item Count

One clever use of `:has()` combined with `:nth-child()` enables quantity queries -- styling a list or grid differently based on how many items it contains. This solves a real design problem: a two-item testimonials section should look different from a six-item one.

```css
/* One or two items: single column */
.testimonials-grid {
  display: grid;
  grid-template-columns: 1fr;
}

/* Three or more items: two columns */
.testimonials-grid:has(.testimonial:nth-child(3)) {
  grid-template-columns: repeat(2, 1fr);
}

/* Six or more items: three columns */
.testimonials-grid:has(.testimonial:nth-child(6)) {
  grid-template-columns: repeat(3, 1fr);
}
```

This pattern is particularly useful for CMS-driven content where editors control how many items appear. The layout adapts without any JavaScript or conditional template logic.

## Page-Level State Management

`:has()` is not limited to component-level styling. You can use it at the document level to manage page-wide state. A common example is locking the page scroll when a modal or dialog is open:

```css
body:has(dialog[open]) {
  overflow: hidden;
}
```

When a `<dialog>` element has the `open` attribute, the body's overflow is hidden. When the dialog closes and the attribute is removed, scrolling resumes. No JavaScript needed to toggle a class on the body.

Another application is adapting the page layout based on sidebar state:

```css
.page-wrapper:has(.sidebar.expanded) .main-content {
  margin-left: 280px;
  transition: margin-left 0.3s ease;
}
```

The main content area shifts when the sidebar is expanded, which you control simply by adding or removing the `.expanded` class on the sidebar element itself.

## Styling Previous Siblings

This is a subtle but powerful ability. CSS traditionally works downwards and inwards -- you can select descendants and following siblings, but not preceding siblings. With `:has()` and the adjacent sibling combinator, you can effectively reverse this:

```css
h2:has(+ p) {
  margin-bottom: 0.5rem;
}
```

This selects `h2` elements that are immediately followed by a `p` element, giving them a tighter bottom margin. A heading followed by a subheading might get different spacing from a heading followed directly by a paragraph. Previously, this required JavaScript or extra wrapper elements.

A practical design example: adjusting heading style when it leads into a blockquote:

```css
h3:has(+ blockquote) {
  border-bottom: 2px solid currentColor;
  padding-bottom: 0.75rem;
}
```

This creates a visual connection between the heading and the quote that follows, with no extra HTML.

## Combining :has() with :not() for Exclusion Patterns

`:has()` composes well with `:not()`. One powerful pattern is styling elements that do NOT contain something -- the absence of a feature rather than its presence:

```css
.card:not(:has(img)) {
  max-width: 480px;
  text-align: center;
}

.button:not(:has(.icon)) {
  padding: 0.75rem 2rem;
}

.button:has(.icon) {
  padding: 0.75rem 1.25rem;
  gap: 0.5rem;
}
```

Buttons with icons get tighter horizontal padding because the icon provides visual weight. Text-only buttons get more padding. This logic lives entirely in CSS, which means your component JavaScript stays simpler.

## Performance Considerations

One concern developers raise about `:has()` is performance. Because it requires the browser to look at an element's children while evaluating styles for a parent, it can be more expensive than simple descendant selectors.

In practice, the impact is minimal for typical web pages. The recommendation is to be specific in your `:has()` selectors -- avoid combinations like `*:has(*)` that require the browser to check every element on the page. Use specific class names and limit the depth of nesting in your `:has()` arguments.

For business websites -- landing pages, service sites, portfolio pages, the kind of projects built for small and medium businesses across Cyprus and beyond -- performance from `:has()` is not a concern worth worrying about. The selector is efficient enough for real-world use.

## Replacing JavaScript: When It Makes Sense

The examples above show cases where `:has()` can replace JavaScript entirely. But it is worth being clear about when that trade-off is right.

CSS `:has()` is appropriate when you want to change visual presentation based on structural content. It is not a logic engine -- it cannot store data, perform calculations, or trigger side effects beyond visual changes. If you need to update content, call an API, or store state in localStorage, you still need JavaScript.

The right mental model is: anything that was previously a JavaScript class toggle on a parent element, based on the state of a child element, is now a candidate for `:has()`. That covers form validation display, modal states, layout variations, and navigation highlighting -- a wide range of common patterns.

## Practical Takeaways

Here are the patterns most worth adding to your CSS toolkit today:

**Form field groups:** Use `.form-group:has(input:user-invalid)` to propagate validation state to the container. This enables cohesive error styling across the label, wrapper, and input with no JavaScript.

**Conditional card layouts:** Use `.card:has(img)` to switch between text-only and media layouts. Keep one clean HTML structure and let CSS handle the variation.

**Page scroll lock:** Use `body:has(dialog[open])` to lock scrolling when modals open. This is simpler and more reliable than toggling a class through JavaScript.

**Navigation parent highlighting:** Use `li:has(a.active)` to style the parent list item rather than just the link. This gives you more control over hover areas and visual treatments.

**Quantity queries:** Combine `:has()` with `:nth-child()` to adjust grid layouts based on item count. CMS-driven sections adapt automatically.

The CSS `:has()` selector is one of the most practically useful additions to CSS in recent years. It closes a gap that pushed many developers towards JavaScript for styling concerns, and it enables cleaner, more maintainable stylesheets. Starting using it today is low-risk: fallbacks are easy to write, support is broad, and the patterns it enables are immediately useful.

[Web Design Paphos](https://webdesignpaphos.github.io/) helps businesses in Cyprus build fast, modern websites.
