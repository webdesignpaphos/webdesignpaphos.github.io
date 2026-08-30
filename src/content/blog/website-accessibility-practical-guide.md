---
title: "Website Accessibility in 2026: A Practical Guide for Business Owners"
description: "WCAG 2.2 and the European Accessibility Act are now in force. Learn what your website needs to be accessible, compliant, and open to every customer."
author: "Web Design Paphos"
date: "2026-08-30"
category: "Web Design"
readTime: "9 min read"
---

Every day, people with visual impairments, mobility limitations, hearing difficulties, or cognitive differences try to visit your website. If your site was not built with accessibility in mind, many of them will encounter walls: buttons they cannot click with a keyboard, images with no description, forms that break without a mouse, videos with no captions.

That is a missed opportunity and, since June 2025, it is also a legal risk for businesses across the EU.

The European Accessibility Act (EAA) came into full effect on 28 June 2025. Every EU member state has transposed the directive into national law. If your business provides digital products or services to EU customers and you have more than 10 employees or more than 2 million euros in annual turnover, compliance is now mandatory. Fines vary by country, from 60,000 euros in Ireland to close to 900,000 euros in Sweden, and the first lawsuits under the EAA were filed in France in November 2025.

But this article is not really about legal risk. It is about building a better website. Accessibility improvements almost always make a site clearer, faster, and easier to use for everyone. This guide explains what the current standard requires, where most websites fall short, and the practical steps you can take starting today.

## Understanding WCAG 2.2: The Standard That Applies Now

The Web Content Accessibility Guidelines (WCAG) are the international framework for website accessibility, published by the World Wide Web Consortium (W3C). In 2026, WCAG 2.2 is the active and enforceable version, referenced by the European Accessibility Act, the UK Equality Act, and the US Americans with Disabilities Act.

WCAG 2.2 defines three compliance levels:

- **Level A** covers the most critical barriers and is the bare minimum
- **Level AA** is the standard required by most regulations worldwide
- **Level AAA** is the highest level, typically applied to specialist accessibility services

For the vast majority of business websites, Level AA is the target. It is achievable without complex engineering and addresses the accessibility barriers that affect the widest range of users.

### What WCAG 2.2 Adds Over Previous Versions

WCAG 2.2 keeps all requirements from WCAG 2.1 and adds nine new success criteria. The most relevant for a typical business website are:

**Focus Appearance (Level AA)**
The keyboard focus indicator, the visible outline that shows which element is currently active, must meet minimum size and contrast requirements. The focused area must be at least the perimeter of the component multiplied by 2 CSS pixels, with a contrast ratio of at least 3:1 between the focused and unfocused states. Many websites remove focus styles entirely for visual reasons. That approach now fails WCAG 2.2.

**Target Size (Level AA)**
Interactive targets, including buttons, links, and icon controls, must be at least 24 by 24 CSS pixels, or have sufficient offset spacing from adjacent targets. Tiny touch targets create problems for users with motor impairments and for mobile users in general. While 24x24 is the floor set by WCAG, 44x44 CSS pixels is the widely recommended comfortable minimum for mobile interfaces.

**Dragging Movements (Level AA)**
Any feature that requires dragging, such as sliders, sortable lists, image carousels, or map controls, must also offer a single-click or tap alternative. Users with limited hand control cannot perform sustained drag actions. If your contact form has a draggable map pin, there must be a text address input as an alternative.

**Redundant Entry (Level A)**
Forms should not ask users to re-enter information they have already provided earlier in the same session. A checkout that asks for a billing address after the user has entered a delivery address to the same location, or a registration form that asks for an email after you have already verified it, fails this criterion.

**Accessible Authentication (Level AA)**
Login processes that rely solely on cognitive tests, such as image puzzles, pattern matching, or complex CAPTCHAs, create barriers for users with cognitive impairments. At least one authentication method must not depend on these tests. Using email magic links, passkeys, or password managers satisfies this criterion.

## The Most Common Accessibility Failures

Automated accessibility scanners consistently flag the same categories of failure across most websites. Understanding these recurring issues is the fastest way to prioritise your improvements.

### Colour Contrast

Low contrast between text and its background is the most widespread accessibility failure by volume. WCAG 2.2 Level AA requires:

- A contrast ratio of at least **4.5:1** for normal text (below 18pt regular or 14pt bold)
- A contrast ratio of at least **3:1** for large text (18pt regular or 14pt bold and above)
- A contrast ratio of at least **3:1** for user interface components such as input borders, icons, and focus indicators

Common failures include light grey body text on white backgrounds (often around 2:1 to 3:1), placeholder text inside form fields rendered in a faint grey, and CTA buttons where both the text and background are medium-intensity colours that create insufficient contrast between them.

The WebAIM Contrast Checker at webaim.org/resources/contrastchecker lets you paste two hex colour codes and see the ratio and pass/fail result immediately. The TPGi Colour Contrast Analyzer is a desktop application that lets you pick colours directly from your screen using a pipette tool. Both are free.

### Missing or Insufficient Alt Text

Alternative text (alt text) is the description attached to an image in its HTML. Screen readers read this aloud for users who cannot see the image. When alt text is absent, a screen reader often reads out the raw filename, something like "IMG_4892.jpg", which is useless.

The rules are straightforward:

- **Meaningful images** such as product photos, team photos, diagrams, and charts need descriptive alt text that conveys the content or function of the image
- **Decorative images** such as background patterns, visual dividers, and icons used alongside visible text labels should use an empty alt attribute: `alt=""`
- **Functional images** such as a logo that links to the homepage or an icon-only button need alt text that describes the action or destination, not just the visual appearance

A logo that links to the homepage should read `alt="Company Name homepage"` rather than `alt="company logo"`. The screen reader user needs to know where the link goes.

### Missing Form Labels

Every form input must have a visible, programmatically associated label. Using placeholder text as a substitute fails accessibility standards for two reasons: placeholder text disappears as soon as the user starts typing, and placeholder text is frequently ignored or misread by screen reader software.

The HTML fix is simple. Add a `<label>` element and link it to the input using matching `for` and `id` attributes:

```html
<label for="email">Email address</label>
<input type="email" id="email" name="email">
```

If your design does not include visible labels, you can visually hide the label while keeping it present in the HTML for screen readers. This satisfies WCAG without changing the visual appearance of your form.

### Keyboard Navigation

Every interactive element on a page must be reachable and operable using only a keyboard. This includes navigation menus, dropdown menus, modals, carousels, accordions, date pickers, and any custom widget.

The basic keyboard interaction model uses:
- **Tab** to move forward through interactive elements
- **Shift+Tab** to move backward
- **Enter** or **Space** to activate buttons and links
- **Arrow keys** to navigate within components such as dropdowns and radio groups

Common keyboard failures include: focus becoming invisible or impossible to track (often caused by `outline: none` in CSS), dropdown menus that open on hover but cannot be reached by keyboard, and modals that open but do not trap focus inside them, causing Tab to send the user to content behind the modal.

Test this yourself: set your mouse aside, open your website, and navigate through your main user journey using only a keyboard. Anywhere you get stuck is a failure.

### Missing Page Language Declaration

Every HTML document should declare the language of the page in the opening tag:

```html
<html lang="en">
```

This single attribute tells screen readers which language pronunciation rules to apply. Without it, a screen reader set to English will mispronounce content on a Greek-language page, and vice versa. For multilingual businesses in Cyprus or across the EU, this is especially important. It is a one-line fix and one of the most commonly missed requirements.

### Videos Without Captions

Any video with meaningful spoken dialogue or audio information must have accurate captions. Auto-generated captions from YouTube and Vimeo are a starting point but frequently contain errors severe enough to alter meaning. If your business publishes video testimonials, explainer videos, or product demonstrations, editing those auto-generated captions is a worthwhile investment.

## Free Tools to Test Your Website

Automated tools give you a quick baseline and identify structural issues efficiently, but they catch only around 30 to 40 percent of real accessibility problems. Manual testing is required for complete coverage.

### WAVE by WebAIM

Install the WAVE browser extension for Chrome or Firefox. Open any page on your site and click the extension icon. WAVE overlays icons directly on your page: red icons mark errors, yellow icons flag alerts, and green icons highlight structural elements such as headings and landmarks. It is visual and approachable for non-developers.

### axe DevTools

The axe browser extension (free version available for Chrome and Edge) is favoured by developers for its zero false positive policy: every issue it reports is a genuine violation. It integrates with the browser developer tools and produces a list of violations with direct references to the relevant WCAG success criteria.

### Google Lighthouse

Built into Chrome DevTools. Open DevTools, select the Lighthouse tab, choose the Accessibility category, and run the audit. Lighthouse returns a score from 0 to 100 alongside a list of failing criteria with links to documentation. Running Lighthouse on the same pages over time gives a useful progress metric, although the score alone does not indicate full compliance.

### Manual Keyboard and Screen Reader Testing

After running automated scans, test keyboard navigation manually as described above. For screen reader testing, NVDA is free for Windows and VoiceOver is built into macOS and iOS. Turn on the screen reader and navigate to your homepage. The experience reveals gaps that no automated tool can detect, including confusing reading order, unlabelled groups of controls, and poorly managed focus after interactions.

## The Business Case for Accessibility

Around 15 to 20 percent of the global population lives with some form of disability. That represents a significant portion of any potential customer base, and websites that exclude them also exclude their spending.

Beyond that, accessibility improvements tend to raise the quality of the experience for all users:

- Clear heading structure and descriptive links help every user scan and navigate content
- High contrast text is easier to read in bright sunlight or on low-quality screens
- Keyboard-accessible interfaces benefit power users who prefer keyboards alongside those who cannot use a mouse
- Well-labelled forms reduce confusion and abandonment across all user groups

A Forrester Research report found that investments in accessibility and user experience produce a return of approximately 100 dollars for every 1 dollar invested. Fewer support requests, lower bounce rates, and higher form completion rates are measurable business outcomes.

## Where to Start This Week

If your website has never been through an accessibility review, begin here:

1. Run a Lighthouse accessibility audit on your homepage and your most important landing page
2. Run WAVE on the same pages and note the red error icons
3. Fix all colour contrast failures using the WebAIM Contrast Checker to verify your choices
4. Add descriptive alt text to all meaningful images and empty alt attributes to decorative ones
5. Add visible, programmatically linked labels to every form field
6. Navigate your entire site using only a keyboard and note where you encounter problems
7. Add `lang="en"` (or the appropriate language code) to every page's HTML element
8. Add captions to any video content

After addressing those seven categories, you will have resolved the highest-impact, most frequently cited accessibility failures. Then continue outward to interior pages, product listings, and any interactive features such as booking forms or calculators.

Accessibility is not a one-time project. Every new image, form, or page you add carries the same requirements. The most practical approach is to build accessibility into your content and development process from the start, making it a regular checkpoint rather than a periodic audit.

[Web Design Paphos](https://webdesignpaphos.github.io/) helps businesses in Cyprus build fast, modern websites.
