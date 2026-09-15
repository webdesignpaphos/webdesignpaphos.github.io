---
title: "How to Optimise Images for a Faster Website in 2026"
description: "A practical guide to WebP, AVIF, lazy loading and responsive images that cut page weight and speed up your website."
author: "Web Design Paphos"
date: "2026-09-09"
category: "Web Design"
readTime: "8 min read"
---

Images are almost always the heaviest assets on a web page. They eat bandwidth, slow down load times, and push your Largest Contentful Paint (LCP) score into the red. Yet most small business websites are still serving bloated JPEGs at full resolution to a visitor on a mobile phone with an average connection.

The good news: fixing your images is one of the highest-return investments you can make in website performance. Done properly, it can reduce your total page weight by 60 to 80 percent and shave two to four seconds off your LCP. This guide covers every layer of image optimisation, from picking the right file format to writing responsive image markup, with specific tools and numbers you can act on today.

## Why Images Are Your Biggest Performance Problem

According to Google's Web Vitals guidance, images are consistently the single largest contributor to page weight on most websites. A typical business homepage might load 10 to 20 images totalling 3 to 5 MB when left unoptimised. A tourist on a rooftop in Paphos, loading your site on a 4G connection with a mid-range Android phone, is going to wait. And they will leave.

The core issues tend to be the same across most sites:

- Photos exported from a camera or Photoshop at maximum quality
- Images sized for desktop, served unchanged to mobile screens
- No use of modern compressed formats
- Hero images given the same lazy treatment as footer thumbnails
- No explicit dimensions declared, causing layout shift

Each of these has a straightforward fix.

## Choosing the Right Image Format

The biggest single gain comes from format selection. Most websites are still defaulting to JPEG or PNG for every image. In 2026, that is leaving enormous file size savings on the table.

### AVIF: The New Standard for Photographs

AVIF (AV1 Image File Format) is the most efficient image format available for general web use. It achieves roughly 50 percent smaller file sizes compared to JPEG at equivalent visual quality. Browser support now sits at approximately 94.9 percent globally, which means only a tiny fraction of visitors cannot load it natively.

For photographs, product images, and any shot with rich colour detail, AVIF should be your primary format. A hero photograph that weighs 800 KB as a JPEG might come in at 380 to 400 KB as AVIF at the same perceptual quality.

### WebP: The Reliable Workhorse

WebP has been available for years and now enjoys 96.4 percent browser support. It delivers 25 to 35 percent smaller files than JPEG, with excellent quality at 75 to 85 compression settings. Use WebP as your fallback for any browser that cannot handle AVIF.

The ideal workflow is to serve AVIF first, WebP second, and JPEG as a last resort. This ensures every visitor gets the smallest file their browser can handle, without any quality loss they would notice.

### When to Keep JPEG and PNG

JPEG remains fine as a final fallback in the picture element. PNG is still the right choice for graphics with transparency, logos on white backgrounds, and images with hard edges where compression artefacts would be obvious. For those use cases, PNG stays. For everything else, move to AVIF and WebP.

For icons and simple graphics, consider SVG instead of any raster format. An SVG logo scales to any size with zero file size penalty and often weighs under 10 KB.

## Serving Multiple Formats with the Picture Element

The HTML `<picture>` element lets you offer several image formats in priority order. The browser picks the first one it understands.

```html
<picture>
  <source srcset="hero.avif" type="image/avif">
  <source srcset="hero.webp" type="image/webp">
  <img src="hero.jpg" alt="Paphos seafront at sunset" width="1200" height="675">
</picture>
```

In this example, a Chrome or Firefox user gets the AVIF version. A browser on an older device that does not yet support AVIF gets the WebP. An extremely old browser gets the JPEG. No JavaScript is required. The browser handles everything natively.

This pattern should be your default for every meaningful image on the page. It costs nothing extra at build time and consistently delivers smaller files to every visitor.

## Responsive Images with srcset and sizes

Format alone is not enough. If you serve a 1400-pixel-wide image to a visitor on a 390-pixel-wide phone screen, the browser downloads a file three to four times larger than it needs. This is wasted bandwidth and wasted load time.

The `srcset` attribute tells the browser what sizes of an image are available. The `sizes` attribute tells it how wide the image will actually render at different viewport widths. The browser does the arithmetic and downloads only the file it needs.

```html
<img
  srcset="product-400.webp 400w, product-800.webp 800w, product-1200.webp 1200w"
  sizes="(max-width: 600px) 100vw, (max-width: 1024px) 50vw, 600px"
  src="product-800.webp"
  alt="Handmade ceramic bowl"
  width="800"
  height="600"
  loading="lazy"
>
```

For most content images, providing three width variants covers the range of devices: 400 pixels for small phones, 800 pixels for tablets and mid-size laptops, and 1200 pixels for large screens. For full-bleed hero images, add a 1600 or 2000 pixel variant to cover high-resolution desktop monitors.

When srcset and sizes are properly implemented, mobile visitors typically download 50 to 70 percent less image data than they would from a single full-size file.

## Lazy Loading vs. Fetch Priority

Not all images are equal on the page, and the browser should not treat them as if they are.

### Which Images Should Load Immediately

Your hero image, the largest image near the top of the page, is almost certainly your LCP element. It is the most important image on the page and it needs to load as fast as possible.

Do not add `loading="lazy"` to your hero image. This is one of the most common mistakes developers make, and it can push LCP scores up by 20 to 30 percent. Instead, use `fetchpriority="high"` to signal to the browser that this image is critical.

```html
<img
  src="hero.webp"
  alt="..."
  width="1200"
  height="600"
  fetchpriority="high"
>
```

### Which Images Should Load Lazily

Everything below the fold can safely use `loading="lazy"`. This tells the browser to skip downloading those images until the visitor scrolls close to them, which speeds up the initial page load significantly.

```html
<img
  src="team-photo.webp"
  alt="Our team"
  width="600"
  height="400"
  loading="lazy"
>
```

A good rule of thumb: the first one or two images visible without scrolling should never be lazy loaded. Everything else should be.

## Image Compression Tools You Should Be Using

You do not need expensive software to compress images properly. Several excellent free tools handle the job.

### Squoosh

[Squoosh](https://squoosh.app/) is a free, browser-based image compressor built by Google. It runs entirely in your browser using WebAssembly, so your images never leave your device. It supports AVIF, WebP, JPEG, and PNG, and gives you side-by-side quality previews so you can see exactly what you are trading for file size.

For AVIF, aim for a quality setting of 60 to 70. For WebP, 75 to 85. For JPEG fallbacks, 80 to 85. At those settings, the compression is essentially invisible to the human eye at normal viewing sizes.

### TinyPNG

[TinyPNG](https://tinypng.com/) is the easiest option if you want to compress quickly without any configuration. Drag your images in, download the compressed versions. It handles PNG and JPEG well and supports WebP. The free tier allows up to 20 images per session with a 5 MB limit per file.

TinyPNG uploads files to its servers for processing, so avoid using it with sensitive or unpublished imagery. For public photos and general site assets, it is perfectly convenient.

### Cloudinary and ImageKit for High-Traffic Sites

If your site handles significant traffic or you manage a large image library, a dedicated image CDN pays for itself quickly. Tools like [Cloudinary](https://cloudinary.com/) and [ImageKit](https://imagekit.io/) serve images from a global network of edge servers, automatically convert formats based on the visitor's browser, resize on the fly, and apply compression without any manual intervention.

The workflow becomes: upload the original high-resolution source, and the CDN handles every variant, every format, and every responsive size automatically. For e-commerce sites or portfolios with hundreds of product images, this eliminates a huge amount of manual work.

## Dimensions, Aspect Ratios and Layout Shift

One detail that is easy to overlook: always declare explicit `width` and `height` attributes on every `<img>` element.

When the browser does not know how large an image will be before it downloads, it cannot reserve space for it in the layout. The result is Cumulative Layout Shift (CLS): text and buttons jump around as images load, which both frustrates visitors and damages your Core Web Vitals score.

Adding width and height attributes allows the browser to calculate the aspect ratio and reserve the correct space immediately, even before the image arrives.

```html
<img src="photo.webp" alt="..." width="800" height="450" loading="lazy">
```

The values you declare do not have to match the rendered pixel size exactly. What matters is that the aspect ratio is correct. The browser applies CSS to scale the image as needed while keeping the reserved space proportionally accurate.

## Putting It All Together: A Practical Checklist

Before considering your images fully optimised, run through this list for every page on your site.

**Format and compression:**
- Convert photographs to AVIF (primary) and WebP (fallback), with JPEG as a last resort
- Use the `<picture>` element to serve the right format to every browser
- Compress AVIF at 60 to 70 quality, WebP at 75 to 85
- Keep PNG only for graphics with transparency or hard edges
- Use SVG for logos and icons

**Responsive delivery:**
- Provide at least three width variants via srcset for content images
- Add a 1600 or 2000 pixel variant for full-bleed hero images
- Include accurate `sizes` attribute so the browser picks the right variant

**Loading strategy:**
- Apply `fetchpriority="high"` to the hero image, remove `loading="lazy"` from it
- Apply `loading="lazy"` to all images below the fold

**Layout stability:**
- Declare explicit `width` and `height` on every `<img>` element

**Tooling:**
- Use Squoosh for manual compression with full format control
- Use TinyPNG for quick bulk compression of standard assets
- Consider Cloudinary or ImageKit if you manage a large or frequently changing image library

Running through this checklist on an existing website typically reduces total image payload by 60 to 80 percent. The effect on load times, bounce rates, and Core Web Vitals is substantial, especially for visitors on mobile connections, which account for the majority of web traffic in Cyprus and across most of Europe.

Image optimisation is one of those tasks that feels technical but pays off in very concrete terms: fewer visitors leaving before the page loads, better search rankings, and a site that feels genuinely fast rather than grudgingly functional.

[Web Design Paphos](https://webdesignpaphos.github.io/) helps businesses in Cyprus build fast, modern websites.
