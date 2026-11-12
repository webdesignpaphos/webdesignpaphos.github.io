---
title: "Design Tokens: The Secret to a Consistent, Easy-to-Maintain Website"
description: "Design tokens let you define your brand colours, fonts, and spacing once and use them everywhere. Here's how to use them effectively."
author: "Web Design Paphos"
date: "2026-11-12"
category: "Web Design"
readTime: "8 min read"
---

If you have ever updated your website's brand colour only to discover it had been used in forty different places, some of which you missed, you already understand the problem that design tokens solve.

Design tokens are one of the most practical ideas to come out of modern web development. They are not a visual trend or a complicated framework. They are simply a disciplined way of storing your brand decisions -- colours, fonts, spacing, border radii, shadows -- so that your entire website stays in sync automatically when anything changes.

This guide explains what design tokens are, why they matter for real businesses, and how to implement them on your own site with nothing more than standard CSS.

## What Are Design Tokens?

A design token is a named variable that holds a single design decision. Instead of writing the hex code `#2563EB` in sixty places across your stylesheet, you create one token called `--color-brand-primary` and use that everywhere. When the colour needs to change, you update the token once. Everything using it updates instantly.

Tokens can store:

- **Colours** -- brand palette, text colours, background shades, border colours
- **Typography** -- font families, font sizes, font weights, line heights
- **Spacing** -- padding, margin, gap values used across components
- **Shape** -- border radius values for buttons, cards, and inputs
- **Shadows** -- box shadow styles for depth and elevation
- **Motion** -- transition durations and easing curves

The idea is not new, but support in browsers has matured significantly. CSS custom properties (also called CSS variables) give you native design tokens in every modern browser with no build tools, no JavaScript frameworks, and no dependencies required.

## Why Design Tokens Matter for Business Websites

For a small or medium-sized business, design tokens solve three real problems.

### Branding Consistency

When a website is built without a token system, design decisions scatter across dozens of files. Buttons might use slightly different shades of your brand colour. Headings on the blog page might use a different font weight than headings on the services page. These inconsistencies seem minor in isolation, but together they make a site feel unpolished and untrustworthy.

Tokens enforce a single source of truth. Every button, heading, and card pulls from the same set of values. The site looks coherent because it is coherent.

### Faster Maintenance

Rebrand exercises that once required a developer to spend days hunting through stylesheets now take minutes. Adjust the tokens, and the update propagates everywhere. The same logic applies to seasonal colour changes, accessibility fixes, and design refreshes.

### Dark Mode and Theming Without Duplication

Adding dark mode without tokens usually means duplicating large chunks of CSS with slightly different colour values. With semantic tokens (more on this below), you redefine only the tokens under a dark-mode condition. Your components need no changes at all.

## The Three Tiers of Design Tokens

Well-structured token systems use three layers. Each layer references the one below it, and components only ever reference the top layer. This keeps things flexible and prevents components from becoming tightly coupled to raw colour values.

### Tier 1 -- Primitive Tokens

Primitive tokens are raw values with no meaning attached. They are your palette.

```css
:root {
  /* Colour palette */
  --blue-500: #2563EB;
  --blue-600: #1D4ED8;
  --blue-700: #1E40AF;
  --grey-100: #F3F4F6;
  --grey-800: #1F2937;
  --grey-900: #111827;
  --white: #FFFFFF;

  /* Type scale */
  --size-sm: 0.875rem;
  --size-base: 1rem;
  --size-lg: 1.125rem;
  --size-xl: 1.25rem;
  --size-2xl: 1.5rem;
  --size-4xl: 2.25rem;

  /* Spacing scale */
  --space-1: 0.25rem;
  --space-2: 0.5rem;
  --space-4: 1rem;
  --space-6: 1.5rem;
  --space-8: 2rem;
  --space-12: 3rem;
}
```

Primitive tokens are never used directly in components. They are the raw ingredients.

### Tier 2 -- Semantic Tokens

Semantic tokens assign meaning. They reference primitive tokens and answer the question: what is this value for?

```css
:root {
  /* Colour semantics */
  --color-brand: var(--blue-500);
  --color-brand-hover: var(--blue-600);
  --color-text-primary: var(--grey-900);
  --color-text-muted: var(--grey-800);
  --color-background: var(--white);
  --color-surface: var(--grey-100);
  --color-border: var(--grey-100);

  /* Typography semantics */
  --font-heading: 'Inter', sans-serif;
  --font-body: 'Inter', sans-serif;
  --text-heading-lg: var(--size-4xl);
  --text-heading-sm: var(--size-2xl);
  --text-body: var(--size-base);
  --text-small: var(--size-sm);

  /* Spacing semantics */
  --spacing-section: var(--space-12);
  --spacing-card: var(--space-6);
  --spacing-button: var(--space-4);
}
```

Notice that the semantic token `--color-brand` does not say "blue". This matters. If you later rebrand to green, you change the primitive token it points to and the name `--color-brand` still makes perfect sense.

### Tier 3 -- Component Tokens (Optional)

For larger projects, you can go one level further with component-scoped tokens. These are useful when a specific component has predictable variations.

```css
.button {
  --button-bg: var(--color-brand);
  --button-bg-hover: var(--color-brand-hover);
  --button-text: var(--white);
  --button-padding: var(--spacing-button);
  --button-radius: 0.375rem;

  background-color: var(--button-bg);
  color: var(--button-text);
  padding: var(--button-padding) calc(var(--button-padding) * 2);
  border-radius: var(--button-radius);
  transition: background-color 150ms ease;
}

.button:hover {
  background-color: var(--button-bg-hover);
}
```

The benefit of component tokens is that you can override a component locally without touching the global semantic layer. A "danger" button variant just redefines `--button-bg` to your error colour token.

## Implementing Dark Mode with Tokens

Dark mode is where the semantic token approach delivers its biggest payoff. Because components reference semantic tokens, all you need to do is redefine those semantic tokens for the dark context.

```css
@media (prefers-color-scheme: dark) {
  :root {
    --color-text-primary: var(--grey-100);
    --color-text-muted: var(--grey-100);
    --color-background: var(--grey-900);
    --color-surface: var(--grey-800);
    --color-border: var(--grey-800);
  }
}
```

That is the entire dark mode implementation. No component CSS needs to change. Every heading, paragraph, card, and form input updates automatically because they all consume the same semantic tokens.

You can also support a manual toggle using a data attribute on the `<html>` element:

```css
[data-theme="dark"] {
  --color-text-primary: var(--grey-100);
  --color-background: var(--grey-900);
  /* other overrides... */
}
```

A small JavaScript snippet then toggles `data-theme` between "light" and "dark" when the user clicks a button, and stores the preference in `localStorage`.

## Naming Conventions That Scale

Good token names follow a consistent pattern: `--[category]-[property]-[variant]-[state]`. Not every token needs all four segments, but keeping the category prefix makes long token files much easier to navigate.

Examples of well-named semantic tokens:

- `--color-text-primary` -- the main text colour
- `--color-text-muted` -- secondary or helper text
- `--color-background` -- the page background
- `--color-surface` -- card and component backgrounds (slightly different from the page)
- `--color-border` -- borders and dividers
- `--color-interactive` -- links and interactive elements
- `--color-interactive-hover` -- hover state of interactive elements
- `--spacing-section` -- vertical padding between page sections
- `--spacing-card` -- internal padding inside cards
- `--radius-sm`, `--radius-md`, `--radius-lg` -- border radius scale

One important rule: never name a token after its raw value. A token named `--color-blue` breaks the abstraction the moment your brand moves to teal. A token named `--color-brand-primary` stays meaningful regardless of the actual colour.

## Sharing Tokens Between CSS and JavaScript

A common question is how to pass token values into JavaScript for animations, charts, or dynamically generated elements. The answer is straightforward: read them from the computed style at runtime.

```js
const styles = getComputedStyle(document.documentElement);
const brandColor = styles.getPropertyValue('--color-brand').trim();
```

This keeps JavaScript in sync with your CSS tokens without duplicating values. If you use a charting library like Chart.js, you can pass `brandColor` directly as the dataset colour and it will automatically reflect any theme changes.

## Tokens and External Design Tools

If your website was designed in Figma, Adobe XD, or a similar tool, you can align your design file tokens with your CSS tokens so that the design and the code always match.

Figma's Variables feature (available in paid plans) lets you define the same three tiers: collections for primitives and semantic aliases. When a designer changes a brand colour in Figma, the CSS token and the Figma variable stay in sync.

For teams using Tailwind CSS version 4, the `@theme` directive lets you define tokens that Tailwind automatically converts into utility classes:

```css
@theme {
  --color-brand: #2563EB;
  --color-brand-hover: #1D4ED8;
  --font-sans: 'Inter', sans-serif;
}
```

After this, `bg-brand` and `text-brand` utility classes become available automatically, alongside `hover:bg-brand-hover`.

## A Practical Starting Point for a Business Website

If you are building or refactoring a business website, here is a minimal but complete token setup that covers the most common needs:

```css
:root {
  /* Primitives */
  --brand-100: #DBEAFE;
  --brand-500: #2563EB;
  --brand-600: #1D4ED8;
  --neutral-50: #F9FAFB;
  --neutral-100: #F3F4F6;
  --neutral-700: #374151;
  --neutral-900: #111827;
  --white: #FFFFFF;

  /* Semantics */
  --color-brand: var(--brand-500);
  --color-brand-hover: var(--brand-600);
  --color-text: var(--neutral-900);
  --color-text-muted: var(--neutral-700);
  --color-bg: var(--white);
  --color-surface: var(--neutral-50);
  --color-border: var(--neutral-100);

  /* Typography */
  --font-sans: system-ui, sans-serif;
  --text-sm: 0.875rem;
  --text-base: 1rem;
  --text-lg: 1.125rem;
  --text-xl: 1.25rem;
  --text-2xl: 1.5rem;
  --text-4xl: 2.25rem;
  --leading-tight: 1.25;
  --leading-normal: 1.5;

  /* Spacing */
  --space-2: 0.5rem;
  --space-4: 1rem;
  --space-6: 1.5rem;
  --space-8: 2rem;
  --space-12: 3rem;
  --space-16: 4rem;

  /* Shape */
  --radius-sm: 0.25rem;
  --radius-md: 0.5rem;
  --radius-lg: 1rem;

  /* Shadows */
  --shadow-sm: 0 1px 2px 0 rgb(0 0 0 / 0.05);
  --shadow-md: 0 4px 6px -1px rgb(0 0 0 / 0.1);
}
```

Save this in a `tokens.css` file and import it at the top of your main stylesheet. From this point forward, every component you build references these names and the whole site stays consistent.

## Common Mistakes to Avoid

**Using raw values in components.** If a component says `color: #2563EB` instead of `color: var(--color-brand)`, that component is disconnected from the token system. Auditing for raw values is worth doing before you launch.

**Making tokens too granular.** It is tempting to create a separate token for every pixel value. Resist this. A scale with seven or eight spacing values covers most needs. Too many tokens are harder to maintain than too few.

**Not documenting the token list.** A tokens file without comments becomes confusing quickly, especially if multiple people work on the site. A brief comment next to each semantic token explaining its intended use takes thirty seconds to write and saves hours later.

**Skipping the semantic layer.** Using only primitive tokens (and skipping the semantic layer) means dark mode and theming require touching every component again. The two-minute investment in naming semantic aliases pays off every time you do a design update.

## Why Design Tokens Are Increasingly Relevant

The push toward design tokens comes from the same place as every efficiency improvement in web development: websites are more complex and need to be maintained over longer periods than they used to be.

Businesses in Cyprus and elsewhere are keeping their websites for three to five years on average. Over that time, brands evolve, design trends shift, and accessibility standards become stricter. A codebase built on a solid token foundation adapts to these changes quickly. One built on scattered raw values resists every change with hours of tedious work.

Design tokens are also becoming a bridge between design teams and development teams. Tools like Tokens Studio for Figma can export token files in JSON format that map directly to CSS variables. The gap between what a designer specifies and what a developer implements shrinks considerably.

If your website is approaching a redesign, or if you are starting from scratch, building a token layer into the foundation costs very little time and pays off every day that follows.

[Web Design Paphos](https://webdesignpaphos.github.io/) helps businesses in Cyprus build fast, modern websites.
