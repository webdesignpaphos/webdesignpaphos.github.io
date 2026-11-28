---
title: "Responsive Images: How srcset and sizes Boost Speed and LCP"
description: "Learn how srcset, sizes, and the picture element serve correctly-sized images to every device, cutting load times and improving Core Web Vitals scores."
author: "Web Design Paphos"
date: "2026-11-28"
category: "Web Design"
readTime: "9 min read"
---

One image file served to every visitor sounds simple, but it is quietly hurting your website. A mobile phone downloading a 2400-pixel-wide image intended for a desktop monitor is wasting data, burning battery, and making your page slower than it needs to be. The fix exists in plain HTML and costs nothing: the `srcset` and `sizes` attributes, paired with the `<picture>` element when needed.

This guide explains how these tools work, why they matter for Core Web Vitals, what mistakes to avoid, and how to apply them on any business website today.

## Why One Image for Every Device Is a Problem

Browsers render images at the size your CSS dictates. A hero image in a full-width section displays at roughly 390 pixels wide on a small phone screen and at 1440 pixels wide on a laptop. If you serve a single 1440-pixel image to both visitors, the phone downloads six times more image data than it displays. That wasted data takes time, and time is what Google measures when it calculates your Largest Contentful Paint score.

Largest Contentful Paint, or LCP, measures how long it takes for the biggest visible element on the page to appear. For most business websites that element is a hero image or a banner photo. Google considers anything under 2.5 seconds a good LCP score. Serving an oversized image to mobile visitors is one of the most reliable ways to push that score above 4 seconds and into the "poor" range.

There is a secondary cost too. Mobile data can still be expensive or slow in many parts of the world, including rural Cyprus. A visitor on a 4G connection who hits your page and finds it slow will leave before they read a single word.

## The srcset Attribute: Telling Browsers What You Have

The `srcset` attribute lets you give the browser a list of image files and their pixel widths. The browser picks the most appropriate one based on the screen resolution and the available display width.

Here is a basic example:

```html
<img
  src="hero-800.jpg"
  srcset="
    hero-400.jpg 400w,
    hero-800.jpg 800w,
    hero-1200.jpg 1200w,
    hero-1600.jpg 1600w,
    hero-2400.jpg 2400w
  "
  alt="A view of the Paphos coastline at sunset"
>
```

The `w` after each number is a width descriptor telling the browser the pixel width of that file. The `src` attribute acts as a fallback for very old browsers that do not understand `srcset`, so keep it pointing to a mid-range size.

A common set of breakpoints for a typical business website is 400, 800, 1200, 1600, and 2400 pixels. Full-bleed hero images usually need the 2400-pixel version for large monitors with high-density screens. Product images in a grid can stop at 1200 pixels.

## The sizes Attribute: Telling Browsers How Images Will Display

Here is where most implementations go wrong. Without the `sizes` attribute, the browser has to guess how wide the image will be on screen. It defaults to 100vw, meaning it assumes the image fills the entire viewport. On a 1440-pixel monitor that assumption triggers the download of a 1440-pixel-wide image file even if the image only occupies half the page.

The `sizes` attribute solves this by telling the browser the actual display width at each breakpoint before the CSS has loaded.

```html
<img
  src="hero-800.jpg"
  srcset="
    hero-400.jpg 400w,
    hero-800.jpg 800w,
    hero-1200.jpg 1200w,
    hero-1600.jpg 1600w,
    hero-2400.jpg 2400w
  "
  sizes="
    (max-width: 480px) 100vw,
    (max-width: 900px) 100vw,
    (max-width: 1440px) 1440px
  "
  alt="Hero image"
>
```

Read the `sizes` value as a list of conditions: "if the viewport is 480px or less, the image takes up 100% of it; between 480px and 900px, still 100%; on larger screens, cap it at 1440px." The browser works through the list from top to bottom and uses the first matching condition.

For images that are not full-width, `sizes` becomes even more important. An image in a two-column layout at desktop takes up roughly 50% of the viewport width. Setting `sizes="(min-width: 1024px) 50vw, 100vw"` tells the browser exactly that, so it picks a file roughly half as large as it would otherwise.

## Applying This to Your Hero Image

The hero section is where responsive images deliver the biggest payoff, because it is usually the LCP element. There are three rules for the hero image:

### Never Lazy Load It

The `loading="lazy"` attribute tells the browser to defer downloading an image until the user scrolls close to it. This is excellent for images below the fold. It is disastrous for the hero, because you want that image downloading immediately. Never add `loading="lazy"` to your hero or any image visible on page load.

### Add fetchpriority="high"

The `fetchpriority` attribute tells the browser to treat this image as a high-priority resource. Without it, the browser downloads many resources in parallel and may not start the hero image until several other requests have finished.

```html
<img
  src="hero-800.jpg"
  srcset="hero-400.jpg 400w, hero-800.jpg 800w, hero-1200.jpg 1200w, hero-1600.jpg 1600w"
  sizes="(max-width: 900px) 100vw, 1440px"
  fetchpriority="high"
  alt="Hero image"
>
```

This one attribute can shave several hundred milliseconds off your LCP on slow connections by moving the image fetch to the front of the queue.

### Preload the Hero Image (Optional but Effective)

If you use a `<link rel="preload">` tag in your page `<head>`, make sure it matches the `srcset` and `sizes` from the image tag. If they do not match, the browser will fetch the preloaded file and then fetch the correct file again when it parses the image tag, causing duplicate downloads.

```html
<link
  rel="preload"
  as="image"
  href="hero-800.jpg"
  imagesrcset="hero-400.jpg 400w, hero-800.jpg 800w, hero-1200.jpg 1200w, hero-1600.jpg 1600w"
  imagesizes="(max-width: 900px) 100vw, 1440px"
>
```

## The picture Element: Format Switching and Art Direction

The `<img>` tag with `srcset` and `sizes` handles size-based selection. The `<picture>` element goes further: it lets you serve entirely different file formats and even different image crops depending on the device.

### Serving Modern Image Formats

AVIF is the most efficient image format available in 2026. Files are typically 50% smaller than JPEG at comparable quality. WebP is a step above JPEG and has better browser support than AVIF. JPEG remains the safest fallback for older browsers.

```html
<picture>
  <source
    type="image/avif"
    srcset="hero-400.avif 400w, hero-800.avif 800w, hero-1200.avif 1200w"
    sizes="(max-width: 900px) 100vw, 1440px"
  >
  <source
    type="image/webp"
    srcset="hero-400.webp 400w, hero-800.webp 800w, hero-1200.webp 1200w"
    sizes="(max-width: 900px) 100vw, 1440px"
  >
  <img
    src="hero-800.jpg"
    srcset="hero-400.jpg 400w, hero-800.jpg 800w, hero-1200.jpg 1200w"
    sizes="(max-width: 900px) 100vw, 1440px"
    fetchpriority="high"
    alt="Hero image"
  >
</picture>
```

The browser reads the `<source>` elements from top to bottom and uses the first format it supports. Modern browsers all support AVIF, so nearly every visitor gets the smallest possible file. The JPEG `<img>` at the bottom is for the rare browser that supports neither.

### Art Direction: Different Crops for Different Screens

Sometimes a wide landscape photo that works on desktop looks cramped or unclear when squeezed onto a 390-pixel-wide phone screen. Art direction lets you serve a tighter crop on mobile.

```html
<picture>
  <source
    media="(min-width: 900px)"
    srcset="hero-landscape-1200.webp 1200w, hero-landscape-1600.webp 1600w"
    sizes="1440px"
  >
  <source
    media="(max-width: 899px)"
    srcset="hero-portrait-400.webp 400w, hero-portrait-800.webp 800w"
    sizes="100vw"
  >
  <img src="hero-landscape-800.jpg" alt="Hero image">
</picture>
```

This delivers the full wide crop to desktop visitors and a tighter square or portrait crop to phone visitors, without any JavaScript.

## Practical Image Generation Workflow

Generating multiple image variants by hand would take hours. These tools automate it.

### Squoosh (squoosh.app)

Google's Squoosh is a free browser-based tool that converts and resizes images. You can export AVIF, WebP, and JPEG from one source file at multiple sizes. It is ideal for a small site with a fixed set of images.

### Sharp (Node.js)

For sites built with Astro, Next.js, or any Node-based framework, the Sharp library generates every size and format you need as part of the build process. A short build script can take one source image and produce a dozen optimised variants automatically.

### Cloudinary and Imgix

If your website serves hundreds of images, an image CDN handles everything automatically. You upload one original file. The CDN resizes, converts, and serves the right variant to every visitor based on their device and browser. Both services have free tiers that cover small business sites.

### PageSpeed Insights

Google's PageSpeed Insights at pagespeed.web.dev runs a free audit of your page and flags oversized images under the "Serve images in next-gen formats" and "Properly size images" diagnostics. Run it on your homepage, service pages, and any page with large images.

## Common Mistakes to Avoid

**Missing the sizes attribute.** Without it the browser assumes 100vw for every image and downloads far more data than the image slot requires. Even a rough value is better than nothing.

**Lazy loading above-the-fold images.** Any image visible on first load should not have `loading="lazy"`. Reserve lazy loading for images the user has to scroll to reach.

**Skipping `alt` text.** This is not optional. Screen readers read `alt` text aloud for visually impaired visitors. Search engines use it to understand image content. Write a short, descriptive sentence for every image.

**Forgetting to set width and height.** When you omit the `width` and `height` attributes, the browser does not know the image dimensions until the file has downloaded. The page shifts when the image loads, causing a poor Cumulative Layout Shift score. Set these attributes on every image, even when you resize the image with CSS.

**Only generating a few sizes.** Two or three sizes is better than one, but a full range of four to five variants lets the browser make a genuinely close match for each screen. The file size difference between a 400-pixel and a 1600-pixel JPEG is enormous, so the more precise the selection, the more data you save.

## A Quick Implementation Checklist

Working through a website audit is easier with a clear list. Here are the checks to run on every image:

- Does the image use `srcset` with at least four width variants?
- Does the image have a correct `sizes` value that matches its CSS display width?
- Is the image in AVIF or WebP format, with a JPEG fallback?
- Is the hero image marked with `fetchpriority="high"` and not `loading="lazy"`?
- Do all images have descriptive `alt` text?
- Do all images have explicit `width` and `height` attributes?

If your website is built on a CMS like WordPress, check that your theme and image settings generate responsive image markup automatically. Modern WordPress themes do this by default, but older or heavily customised themes may not.

## The Business Case in One Paragraph

Faster pages convert better. Research from Google consistently shows that every second of additional load time reduces conversion rates. For a local business, that means fewer contact form submissions, fewer phone calls, and fewer bookings from visitors who left before the page finished loading. Responsive images are one of the highest-return improvements you can make to an existing site, because the fix is in HTML markup, not a costly redesign.

[Web Design Paphos](https://webdesignpaphos.github.io/) helps businesses in Cyprus build fast, modern websites.
