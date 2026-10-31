---
title: "OKLCH in CSS: How to Build Website Color Palettes That Actually Work"
description: "Learn how the OKLCH color space fixes the problems with HSL and HEX, and how to build perceptually uniform, accessible color palettes for your website."
author: "Web Design Paphos"
date: "2026-10-31"
category: "Web Design"
readTime: "9 min read"
---

If you have ever picked two colors that looked perfectly balanced on screen, only to find one shade looks noticeably brighter than the other when placed side by side, you have encountered the core problem with how the web has handled color for the past three decades. The old color formats, HEX, RGB, and even HSL, describe colors in ways that do not match how human eyes perceive them. OKLCH is a modern CSS color space that solves this problem. It has been supported in all major browsers since 2023, and in 2026 it has become the standard approach for design systems built to last.

This article explains what OKLCH is, how its syntax works, and how to use it to build a professional, accessible color palette for your website.

## Why Old Color Formats Fall Short

Before diving into OKLCH, it helps to understand the specific problems it fixes.

### The HSL trap

HSL (Hue, Saturation, Lightness) was a big improvement over HEX because its values were human-readable. You can look at `hsl(210, 80%, 50%)` and understand it is a moderately light blue. The problem is that the "lightness" value in HSL is a lie, or at least a simplification. Two colors with identical lightness values, say `hsl(60, 80%, 50%)` (a yellow) and `hsl(240, 80%, 50%)` (a blue), look dramatically different in perceived brightness to the human eye. Yellow registers as much brighter. This means building a consistent, balanced color scale with HSL requires a lot of manual tweaking.

### The gradient muddle

Pick any two saturated colors, say a vivid orange and a vivid purple, and try creating a gradient between them in standard RGB or HSL. The middle of the gradient often turns a muddy gray or brownish color because the interpolation path cuts through the desaturated center of the color space. OKLCH gradients follow a perceptual arc instead, keeping saturation high through the entire transition.

### Accessibility guessing games

Because HSL lightness values do not correspond to perceived brightness, hitting WCAG contrast ratios by eye is unreliable. You often end up checking a contrast tool, adjusting, checking again. OKLCH's lightness axis is perceptually uniform, which means adjusting the L value actually changes how bright the color appears in a predictable, linear way.

## What OKLCH Is and How Its Syntax Works

OKLCH stands for OK (a reference to the Oklab perceptual color model it is built on), L for Lightness, C for Chroma, and H for Hue. It describes every color using three values that map much more closely to how human vision works.

The CSS syntax looks like this:

```css
color: oklch(L C H);
```

Or with an optional alpha channel:

```css
color: oklch(L C H / alpha);
```

### The three axes explained

**Lightness (L)** runs from 0 to 1, where 0 is pure black and 1 is pure white. Unlike HSL, this lightness is perceptually uniform: stepping from 0.4 to 0.5 looks like the same visual jump as stepping from 0.6 to 0.7, regardless of the hue.

**Chroma (C)** controls color saturation. It starts at 0, which is a neutral gray, and increases as the color becomes more vivid. For colors that fit within the sRGB gamut (the range standard monitors can display), chroma typically falls between 0 and about 0.35. Colors beyond this range enter the P3 wide gamut, which newer displays support, giving you access to more vivid shades than HEX or HSL ever allowed.

**Hue (H)** is an angle from 0 to 360 degrees. Red sits near 29 degrees, yellow near 90, green near 145, cyan near 190, blue near 265, and purple near 310. This is similar to HSL's hue wheel but calibrated to perceptual evenness.

A few practical examples:

```css
/* A warm brand red */
color: oklch(0.55 0.20 29);

/* A calm mid-blue */
color: oklch(0.58 0.18 265);

/* A vivid P3 green (beyond sRGB) */
color: oklch(0.70 0.28 145);

/* White */
color: oklch(1 0 0);

/* Black */
color: oklch(0 0 0);
```

## Building a Tonal Color Scale With OKLCH

One of the most powerful uses of OKLCH is building a tonal scale for a brand color: a series of shades from near-white to near-black that stay visually coherent across every step. Frameworks like Tailwind CSS and Radix UI have popularized 11-step scales labeled 50 through 950, and OKLCH is the best tool for creating them.

### The method

Pick your base brand color. Let's say it is a deep teal with an hue of around 195 degrees. Map it to the 500 step on your scale, which is typically the primary use color. Then hold the hue and chroma mostly constant while stepping the lightness value up (toward white) and down (toward black).

```css
:root {
  --teal-50:  oklch(0.97 0.03 195);
  --teal-100: oklch(0.93 0.05 195);
  --teal-200: oklch(0.87 0.09 195);
  --teal-300: oklch(0.78 0.13 195);
  --teal-400: oklch(0.68 0.17 195);
  --teal-500: oklch(0.58 0.19 195);  /* base brand color */
  --teal-600: oklch(0.49 0.18 195);
  --teal-700: oklch(0.40 0.15 195);
  --teal-800: oklch(0.31 0.11 195);
  --teal-900: oklch(0.22 0.07 195);
  --teal-950: oklch(0.14 0.04 195);
}
```

Notice that chroma is also gently reduced at the very lightest and darkest ends. At near-white or near-black, high chroma produces colors that look harsh or unnatural. Tapering it off keeps the scale looking refined.

### Why this beats the HEX approach

If you had tried to build this same scale using HEX values, you would have pulled each color out of a color picker and hoped the steps looked even. In OKLCH, the steps are mathematically even because the lightness values are spaced evenly. The result is a palette that behaves predictably when you apply it to text, backgrounds, borders, and hover states.

## OKLCH and Accessibility

OKLCH's perceptual lightness model makes it significantly easier to build accessible color systems that meet WCAG contrast requirements.

### Predicting contrast ratios

WCAG 2.1 and 2.2 require a contrast ratio of at least 4.5:1 for normal text on a background, and 3:1 for large text or UI components. With HSL, predicting whether two colors will pass this test is difficult because HSL lightness is not perceptually accurate. With OKLCH, colors with an L difference of about 0.4 or more will almost always pass the 4.5:1 threshold, depending on their chroma values. This is not a hard rule, but it gives you a reliable starting point.

### A semantic token approach

Rather than using raw scale values throughout your CSS, define a layer of semantic tokens that assign scale steps to roles:

```css
:root {
  /* semantic surface tokens */
  --surface-primary:    var(--teal-500);
  --surface-hover:      var(--teal-600);
  --surface-subtle:     var(--teal-50);

  /* text on dark backgrounds */
  --text-on-primary:    oklch(0.97 0 0);  /* near white */

  /* text on light backgrounds */
  --text-primary:       oklch(0.15 0 0);  /* near black */
}
```

When you later need a dark mode, you only remap the semantic tokens to different scale steps, rather than hunting through your entire stylesheet.

## OKLCH Gradients

The gradient improvement with OKLCH is one of the most immediately visible benefits. Compare these two approaches:

```css
/* Old approach: gradient through gray */
.old-gradient {
  background: linear-gradient(
    to right,
    hsl(30, 90%, 55%),
    hsl(270, 80%, 55%)
  );
}

/* New approach: vivid arc through hue space */
.new-gradient {
  background: linear-gradient(
    in oklch to right,
    oklch(0.65 0.22 55),
    oklch(0.58 0.19 295)
  );
}
```

The keyword `in oklch` inside the `linear-gradient` function tells the browser to interpolate the gradient in OKLCH color space. The result stays vibrant from edge to edge, traveling through the warm and cool hues rather than cutting through gray.

You can also specify the interpolation direction on the hue axis:

```css
background: linear-gradient(
  in oklch shorter hue to right,
  oklch(0.65 0.22 55),
  oklch(0.58 0.19 295)
);
```

The `shorter hue` keyword takes the shortest path around the color wheel. `longer hue` takes the long way round, sweeping through more of the spectrum.

## Browser Support and Fallbacks

OKLCH has full support in Chrome 111+, Firefox 113+, and Safari 15.4+. As of late 2026, global browser support sits above 94 percent. For the small fraction of users on older browsers, a fallback is straightforward:

```css
.button-primary {
  /* Fallback for older browsers */
  background-color: #1a7a8c;

  /* OKLCH for modern browsers */
  background-color: oklch(0.55 0.10 195);
}
```

Modern browsers will read both declarations and use the last one they understand. For more critical use cases, you can use the `@supports` rule:

```css
.button-primary {
  background-color: #1a7a8c;
}

@supports (color: oklch(0 0 0)) {
  .button-primary {
    background-color: oklch(0.55 0.10 195);
  }
}
```

In practice, for most business websites targeting current users in markets like Cyprus and Western Europe, the fallback is unlikely to ever be needed. But it costs nothing to include.

## Tools for Working With OKLCH

The main challenge with OKLCH is that you cannot pick colors from a standard color picker and get OKLCH values directly. A few tools make the workflow practical:

**oklch.com** is a browser-based color picker built specifically for the format. You can drag sliders for lightness, chroma, and hue, see the resulting CSS value, and check contrast ratios against other OKLCH colors in real time. It also shows a gamut warning if your color exceeds the sRGB range.

**Oklch.io and Evil Martians' OKLCH palette generator** let you enter a base brand color and automatically generate an 11-step tonal scale with evenly spaced lightness values.

**Figma** added support for OKLCH-style perceptual color management in its 2025 updates. You can enable it in the color picker and build palettes that will translate consistently from design to code.

**PostCSS plugins** such as `postcss-oklab-function` can convert OKLCH values to HEX during a build step, which is useful if you want the ergonomics of OKLCH in development but need to support a wider range of browsers in production.

## Practical Implementation Advice

If you are starting a new website build, begin your color work in OKLCH from day one. Define your brand palette as a set of CSS custom properties in your root styles, use semantic token names throughout your component CSS, and check contrast ratios using oklch.com before finalizing your choices.

If you are updating an existing site, you do not need to convert every color at once. A good starting point is to replace your key interactive colors, primary buttons, links, focus states, and form inputs, where consistent, predictable contrast matters most.

Avoid using very high chroma values for large background areas. Highly saturated colors at 0.25 or above are striking in small doses, as on a call-to-action button, but on a background panel or hero section they can cause visual fatigue. Keep backgrounds toward the low end of your chroma scale and save the vivid shades for accents and interactive elements.

When building for dark mode, resist the temptation to simply invert your lightness values. A dark theme works best when midtones are carefully remapped and background steps are shifted to very low lightness, around 0.10 to 0.18, rather than pure black. Adding a tiny amount of chroma, around 0.03 to 0.05, to your dark background colors keeps them from looking cold and flat.

## Wrapping Up

OKLCH is not a trend that will fade. It is a fundamental improvement in how CSS handles color, backed by decades of perceptual color research and now available in every browser your users are likely to be running. Designers who learn it now will spend less time fighting color tools, build more accessible interfaces by default, and produce palettes that look polished and coherent across every use case.

The shift from HEX to HSL felt like a big step forward in readability. The shift from HSL to OKLCH feels like a similarly significant upgrade in reliability.

[Web Design Paphos](https://webdesignpaphos.github.io/) helps businesses in Cyprus build fast, modern websites with clean, consistent visual design.
