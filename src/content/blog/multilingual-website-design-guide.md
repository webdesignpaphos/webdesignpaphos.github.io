---
title: "Multilingual Website Design: A Practical Guide for Businesses"
description: "How to design a website that works across multiple languages, from layout and language switchers to hreflang, text expansion, and translation tools."
author: "Web Design Paphos"
date: "2026-11-19"
category: "Web Design"
readTime: "9 min read"
---

A website built for one language leaves money on the table. For many businesses, a significant share of their potential customers speaks a different language entirely. Restaurants, hotels, law firms, and service businesses that serve a diverse customer base know this problem first-hand. Building a multilingual website is not simply a matter of running your content through Google Translate and hitting publish. It requires deliberate design decisions at every level: URL structure, layout, typography, language detection, and SEO signals.

This guide walks through every major decision you will face when designing or upgrading a website to serve multiple languages.

## Why Multilingual Websites Demand a Different Design Approach

Most designers treat translation as a content problem, not a design problem. That view costs businesses. A word-for-word translation can break a layout, destroy readability, or confuse users who expect a site tailored to their language and culture, not a translated version of something built for someone else.

The design challenges are real and specific:

- Text in many European languages expands 30 to 50 percent compared to English. Short labels, button text, and navigation items can overflow containers sized for concise English words.
- Some languages, including Arabic, Hebrew, and Persian, flow right-to-left. A layout designed only for left-to-right reading needs structural changes, not just a text-direction toggle.
- Typography varies by script. Fonts that handle Latin characters beautifully may render Cyrillic, Greek, or Arabic characters poorly or not at all.
- Cultural expectations around colour, imagery, and tone differ between markets, sometimes significantly.

Addressing these issues after launch is expensive. The best time to design for multilingual use is before a single line of code is written.

## Choosing Your URL Structure

The URL structure you choose affects both user experience and SEO. Three models exist, each with different trade-offs.

### Country-Code Top-Level Domains (ccTLDs)

Separate domains for each language version, such as example.gr for Greek and example.com for English, give the clearest geographic and linguistic signals to search engines. Users immediately understand which site they are on. The downsides are cost, maintenance overhead, and the fact that domain authority does not transfer between domains. Each version must build its own backlink profile from scratch.

This model works well for large organisations with dedicated regional teams and budgets. For most small and medium businesses, it creates unnecessary complexity.

### Subdomains

A subdomain model uses addresses like gr.example.com or en.example.com. Search engines treat subdomains as separate sites for ranking purposes, which creates the same domain-authority fragmentation problem as ccTLDs. Subdomains are easier to manage than separate domains but offer few advantages over subdirectories for most businesses.

### Subdirectories

The subdirectory approach places language versions within a single domain, using paths like example.com/en/ and example.com/el/. This is the model most SEO professionals recommend in 2026 for small and medium-sized businesses. All pages share the same domain authority. One CMS installation handles everything. Maintenance is simpler.

The main limitation is that subdirectories use language codes rather than country-specific signals. If you need to target both British and American English as genuinely distinct audiences, subdirectories using /en-gb/ and /en-us/ add complexity. For most businesses, this is an edge case.

## Designing the Language Switcher

The language switcher is the navigation control users rely on to find their version of your site. Poor switcher design is one of the most common reasons multilingual websites fail to convert users.

### Placement and Visibility

Put the switcher in the header, in a consistent position across every page. Repeating it in the footer costs nothing and helps users who have scrolled past the top of a page. Do not bury it in a hamburger menu on mobile, where it becomes invisible to anyone who needs it most.

### Label Format

Label each language option in that language, not in English. Display "Ελληνικά" rather than "Greek," and "Deutsch" rather than "German." A Spanish speaker who has just landed on your English site may not read English well enough to find the option labelled in English.

Using flag icons alone is a common mistake. Flags represent countries, not languages. Spanish is spoken in more than 20 countries. French is spoken across four continents. If you use flags, pair them with the language name in that language.

### Destination Behaviour

When a user switches language, they should land on the equivalent page in that language, not on the homepage of the new language version. If a user is reading your services page in English and switches to Greek, they should arrive on your Greek services page. Dropping them at the Greek homepage forces them to navigate all over again, which most users will not bother to do.

This requires that each page has a matching translated version. If a page has not been translated yet, redirecting to the homepage is acceptable as a fallback, but it should be clearly communicated.

### Auto-Detection

Detecting a user's browser language and offering or redirecting to the matching version reduces friction. Implement this carefully: show a notification or prompt rather than silently redirecting, since automatic redirects can confuse users who need a specific language version for professional or accessibility reasons. Always allow users to override the detected language.

## Handling Text Expansion in Your Layout

Translated text almost always takes up more space than English. IBM's internationalisation guidelines, still widely cited, suggest that strings under 10 words can expand by 200 to 300 percent in some European languages. Even moderate expansions of 30 to 50 percent can break fixed-width containers, truncate navigation labels, or force awkward line breaks in headings.

Build flexibility into your layout from the start:

- Use CSS logical properties (`inline-size`, `block-size`) rather than fixed `width` and `height` values for elements that contain text.
- Set `min-width` on buttons and labels rather than a fixed width.
- Test your layouts with longer placeholder text before connecting real translations.
- Use `overflow: hidden` and `text-overflow: ellipsis` only where truncation is explicitly acceptable, not as a catch-all fix.
- Avoid centering long translated strings in narrow columns without testing how they wrap.

Headings deserve special attention. A four-word English heading that fills a single line may become a seven-word translated heading that wraps awkwardly across two lines. Use `hyphens: auto` in CSS for languages that support hyphenation, and test heading sizes in every language version.

## Right-to-Left Language Support

If your audience includes speakers of Arabic, Hebrew, Farsi, or Urdu, you need proper right-to-left (RTL) support. This goes beyond setting `dir="rtl"` on the HTML element, though that is the starting point.

Use CSS logical properties throughout your stylesheet. Properties like `margin-inline-start` and `padding-inline-end` adapt to the text direction automatically. Properties like `margin-left` and `padding-right` do not. Rewriting an existing stylesheet to use logical properties takes effort upfront but pays off when the entire layout mirrors correctly for RTL languages without additional overrides.

Icons with directional meaning, such as arrows, play buttons, and back navigation, should also mirror in RTL context. An arrow pointing right to indicate "next" in an LTR layout should point left in an RTL layout.

Fonts must support the target script. Load appropriate font subsets and test rendering across browsers and devices.

## Typography Across Scripts

A typeface that renders beautifully in English may have poor or no support for Greek, Cyrillic, Arabic, Chinese, or other scripts. When selecting fonts for a multilingual site, check the character coverage for every script you need.

Google Fonts specifies the scripts each font supports. For Latin-based languages, most modern fonts provide full coverage. For Greek, look for fonts that specify "Greek" or "extended Latin" coverage. For scripts like Arabic or CJK (Chinese, Japanese, Korean), specialist fonts are usually required.

Fallback font stacks matter. Define a `font-family` stack that includes a suitable fallback for each script, so even if the primary webfont fails to load, the text remains readable:

```css
body {
  font-family: "Inter", "Noto Sans", "Helvetica Neue", sans-serif;
}
```

Noto Sans and Noto Serif from Google are designed to cover a very wide range of scripts and serve as reliable fallbacks.

## Implementing Hreflang for SEO

Hreflang is the HTML attribute that tells search engines which language and region each page targets. Without it, search engines may show the wrong language version to users, duplicate content penalties may apply, and your language targeting will rely entirely on URL structure.

Each page must include a `hreflang` tag for every language version of that page, including itself. A page available in English, Greek, and Russian needs three tags: one pointing to the English version, one to the Greek version, and one to the Russian version. This reciprocal linking is mandatory. If the Greek page does not include tags pointing back to the English and Russian versions, search engines may ignore the tags entirely.

```html
<link rel="alternate" hreflang="en" href="https://example.com/en/services/" />
<link rel="alternate" hreflang="el" href="https://example.com/el/services/" />
<link rel="alternate" hreflang="ru" href="https://example.com/ru/services/" />
<link rel="alternate" hreflang="x-default" href="https://example.com/en/services/" />
```

The `x-default` tag designates the fallback version shown to users whose language is not specifically targeted.

Common hreflang mistakes include using incorrect language codes (use ISO 639-1 two-letter codes like `el` for Greek, not `gr`), mixing canonical and hreflang settings that contradict each other, and implementing tags on only one side of the reciprocal relationship.

## Translation Versus Localisation

There is a meaningful difference between translating a website and localising it. Translation converts text from one language to another. Localisation adapts the entire experience for a specific audience: currency, date formats, measurement units, images, examples, calls to action, and cultural tone.

A business in Cyprus serving both English-speaking tourists and Greek-speaking locals is a good example of where localisation matters. The English version may emphasise convenience and scenic location for visitors. The Greek version may focus on community, long-term reliability, and local reputation. Both are true, but the emphasis reflects different audience priorities.

For small businesses, full localisation of every page is often impractical. Prioritise the pages with the highest impact: the homepage, services pages, contact page, and any high-traffic landing pages. Machine translation tools like DeepL have improved dramatically and can produce a credible first draft for lower-priority pages. Human review remains essential for any page where tone, accuracy, or trust matters.

## Tools for Managing Multilingual Websites

Several translation management tools integrate with popular CMS platforms:

**Weglot** works as a layer on top of your existing site and handles translation through a visual editor. It supports over 100 languages and integrates with most platforms without requiring structural changes. Plans are priced per language and by word count, which suits businesses starting with one or two additional languages.

**WPML** (WordPress Multilingual Plugin) is the most widely used solution for WordPress sites. It gives granular control over URL structure, translation workflows, and WooCommerce compatibility. The setup is more involved than Weglot but provides deeper integration.

**Polylang** is a free WordPress plugin with a Pro version. It lacks the translation management features of WPML but covers the core multilingual URL and content management needs for simpler sites.

**DeepL** offers an API that connects to most content management and translation workflows, providing machine translation that is significantly more accurate than older tools for European languages.

For custom-built sites, the i18next library is the standard choice for JavaScript applications. It supports interpolation, pluralisation, and language detection, and integrates with React, Vue, and other modern frameworks.

## Performance Considerations

Each language version adds pages, assets, and potentially additional JavaScript for language detection and switching. A few practices keep performance in check:

- Lazy-load translations for languages other than the user's current language. Do not send 50 kilobytes of Greek and Russian strings to every English-speaking visitor on page load.
- Serve font subsets relevant to the active language only. Loading full CJK font files for users who will never see those characters wastes bandwidth.
- Cache translated pages at the CDN level. Translated content changes less frequently than the source language, so long cache TTLs are often appropriate.
- If you use a third-party translation overlay service, benchmark its impact on Core Web Vitals. Some services inject significant JavaScript and can delay the Largest Contentful Paint.

## Testing a Multilingual Website

Before launching, test every language version as a real user would experience it:

1. Navigate to the site with your browser language set to each supported language and verify that detection and redirection work correctly.
2. Switch between language versions from multiple pages and verify that each switch lands on the correct equivalent page.
3. Test all forms, error messages, and confirmation messages in each language. These are frequently overlooked.
4. Check layouts with the longest translated strings on the smallest supported screen size.
5. Validate hreflang implementation using Google Search Console's International Targeting report or a dedicated hreflang validator.
6. Check that meta titles and descriptions are translated for every page, not just the body content.

## A Practical Starting Point

For most small businesses launching their first multilingual version, the simplest effective approach is:

- Choose subdirectory URL structure
- Prioritise translation of the five to ten pages that drive the most traffic or conversions
- Use a human translator for the homepage and services pages, DeepL for the rest
- Implement hreflang with a plugin or CMS feature rather than manually
- Use a visible, correctly labelled language switcher in the header
- Test on a real device in each target language before launch

Expanding to additional languages is straightforward once the structure is in place. Getting the structure right the first time avoids a costly rebuild later.

---

[Web Design Paphos](https://webdesignpaphos.github.io/) helps businesses in Cyprus build fast, modern websites that reach the right audience in the right language.
