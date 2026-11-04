---
title: "Website Empty States: The Overlooked Design Element That Affects Conversions"
description: "Learn how to design effective empty states for every screen on your site, from first-time use to no-results pages and error screens."
author: "Web Design Paphos"
date: "2026-11-04"
category: "Web Design"
readTime: "8 min read"
---

Most businesses spend enormous energy on their homepage, their product pages, and their navigation menus. But there is one part of almost every website that gets designed by accident: the empty state.

An empty state is what a visitor sees when there is no content to display. It shows up when a new user arrives and has not yet added anything to their account, when a search or filter returns zero results, when a cart or inbox is completely empty, or when a connection fails and a page cannot load data.

The instinct is to drop in a message like "No results found" and move on. That instinct costs businesses real conversions. Some usability studies cite that roughly 75% of new users abandon a product within the first week - often because they hit an empty screen and had no idea what to do next. Getting empty states right does not require a major redesign. It requires treating each one as a small, intentional design problem with a clear solution.

## The Four Main Types of Empty States

Before designing, you need to identify which kind of empty state you are dealing with. Each one has a different cause, a different visitor expectation, and a different ideal response.

### First-Time Use

This is the screen a brand-new visitor sees when they arrive with no data yet created. A new account dashboard, an empty project list, a blank portfolio section - all of these are first-time-use empty states.

The visitor here is not frustrated. They are expectant. Your job is to show them what this space will look like when it is working, and give them a clear action to begin. A good first-time-use empty state includes a simple illustration or icon that represents the content that belongs here, a short encouraging headline like "Start by adding your first product," a one-sentence description that tells them what this section does, and a prominent call-to-action button.

Avoid showing a generic "Nothing here yet" message. It says nothing about what goes here or why the user should care.

### No Results

This state appears when a search, a filter, or a category returns zero matches. It is one of the most damaging empty states on any website because it arrives precisely when a visitor is actively trying to find something.

The common mistake is treating this as a dead end. A well-designed no-results state treats it as a redirect opportunity. It should include a clear headline that names the problem ("No results for 'outdoor chairs'"), a suggestion to broaden the search or clear filters, links to popular categories or top-selling products, and ideally a search box pre-filled with the original query so the user can quickly adjust it.

If you run an online shop and your filter combinations can produce zero results, this state alone is worth spending real design time on.

### Error States

Error empty states occur when something goes wrong: a network request fails, an API times out, or the user loses their connection. The page has nothing to display not because there is nothing there, but because it could not be fetched.

Here the visitor needs two things: a plain-language explanation and a way to recover. Consider the difference between these three versions of the same message:

- Weak: "Error 503: Service unavailable"
- Better: "We could not load your orders right now"
- Best: "We could not load your orders right now. This is usually temporary. Try refreshing, or check back in a few minutes."

Always include a retry button where possible. Do not default to sending users back to the homepage unless that is genuinely the most helpful place for them to go.

### Completed and Cleared States

These are the happy empty states: a notifications inbox with everything read, an order that has been fulfilled, a task list where every item is ticked off. The user has done exactly what they were supposed to do.

These screens are frequently overlooked. A short positive message like "All caught up - no new notifications" feels considerably better than a silent blank screen. It acknowledges the user's effort and closes the loop on the interaction.

## Anatomy of a High-Performing Empty State

Whatever the type, effective empty states share the same core elements. Think of these as a template you apply and adjust for each situation.

### The Illustration

Illustrations communicate faster than words and distinguish an empty state from a broken page. A few practical points:

Keep illustrations compact. A good rule of thumb is to constrain them to 200px wide or less on desktop. The illustration supports the message; it does not replace it.

Use spot illustrations rather than full scenes. A single clear icon or simple graphic is decoded faster than a complex, layered scene.

Match your brand style. If your website uses flat, geometric graphics throughout, a detailed photorealistic illustration will feel like it was dropped in from a different site. Tools like [Undraw](https://undraw.co) offer a large library of free, customisable SVG illustrations that you can recolour to match your palette without hiring a designer.

### The Headline

The headline is the first line of text the user reads. It should state the situation plainly and begin to suggest a resolution.

Avoid headlines that make the user feel at fault. "You have not added anything yet" puts the problem on the visitor. "Nothing saved here yet" is neutral. "Save products here to find them quickly later" is better still - it explains the purpose of the section and gives a reason to act.

Write headlines in active, present-tense language that points forward rather than announcing an absence.

### The Description

One to two sentences that expand on the headline. Use this space to briefly explain what this section is for and why filling it benefits the user. Keep it under 40 words. Longer descriptions get skipped entirely.

### The Call to Action

This is the most important element in the whole pattern. Without it, an empty state is a dead end regardless of how well the headline is written.

For first-time use, the call to action is typically the main action that creates content: "Add your first product," "Upload a file," or "Start a project." For no-results states, it is usually a redirect: "Browse our full catalogue," "Clear all filters," or "Try a different search term." For error states, it is a recovery option: "Refresh the page," "Try again," or "Contact support."

Use a single primary button. If you offer two options, make one a primary button and the other a plain text link. Two equally weighted buttons split attention and reduce clicks on both.

## Writing Copy That Works

The tone of your empty state copy should match the context. A cart that is empty during an active shopping session is a different situation from an inbox showing zero messages after a user has cleared everything. Adjust the register accordingly.

A few guidelines that apply across all types:

**Be specific.** "No items in this category" is better than "Nothing here." "No orders from the last 30 days" is better than "No orders found." Specific copy builds trust because it shows the system actually understood what the user was looking for.

**Avoid technical language.** Users do not need to know why something is empty. They need to know what to do next.

**Match sentence length to the situation.** Error states need a brief explanation and a retry button, not a paragraph. First-use states have more room to sell the value of the feature.

**Use plain numbers where relevant.** Some e-commerce platforms show adjacent result counts on a no-results screen: "0 results in Dining Tables - 46 products in Outdoor Furniture." That specificity reduces bounce rate because it tells the user something useful is nearby.

## Common Mistakes to Avoid

### Using One Generic Empty State for Everything

A single "Nothing to show" message gets dropped across every blank screen and treated as done. The problem is that "nothing to show" means completely different things depending on context. A first-time user and a filtered search that returned no matches are having entirely different experiences. Generic states leave both groups without a useful next step.

### Forgetting the Call to Action

An illustration and a headline with no button is decoration, not design. If the user reads your empty state and closes the tab because they do not know what to do next, the state has failed regardless of how well it looks.

### Too Much Text

Empty states that explain at length why something is empty, apologise for it, and then list three options tend to get ignored. Keep the message short and the action obvious.

### Identical States Across Themes and Screen Sizes

An empty state illustration that looks fine on a light background may become invisible in dark mode. An illustration sized for desktop may push the call-to-action button below the fold on mobile. Test every empty state at different screen sizes and in both colour schemes before shipping.

## Accessibility Considerations

Empty states are frequently forgotten during accessibility reviews, but they present specific challenges.

**Screen reader announcements.** When a search is submitted and the results area dynamically clears and shows an empty state, screen reader users need to know the page content changed. Adding `role="status"` or `aria-live="polite"` to the empty state container tells assistive technology to announce the update. Without this attribute, a user may submit a search form, the content area updates silently, and they have no way of knowing anything happened.

**Colour contrast.** Illustrations and muted secondary text in empty states routinely fail contrast requirements. Light grey text on a white background is a very common offender. Aim for at least 4.5:1 contrast ratio for body text and 3:1 for large text. Run your empty state designs through a contrast checker like Stark or Colour Contrast Analyser before launch.

**Focus management.** When a search returns no results, consider programmatically moving keyboard focus to the empty state container or its headline. This saves keyboard-only users from having to navigate past every header and sidebar element to discover that the results area is empty.

## Tools and Patterns to Get Started

If you are building or redesigning a website, adding proper empty states to your planning checklist takes five to ten minutes and saves significant rework later.

**Undraw (undraw.co):** Free, customisable SVG illustrations covering dozens of scenarios including "empty," "search," "no data," and "error." You set the colour and export.

**Error.design:** A curated gallery of real empty state and error screens from live products. Useful for inspiration and for seeing how different industries handle the same patterns.

**Figma UI Kits:** Most established Figma component libraries - including Google Material Design 3 and IBM Carbon Design System - include pre-built empty state components with the anatomy already in place. Using them as a starting point is faster than designing from scratch.

When planning any new website or feature, map out every list, search, dashboard, and inbox on the site. For each one, ask three questions: what does this look like with one item? With zero items? When it fails to load? Designing all three states upfront is far more efficient than patching them after a user complains.

## The Business Case for Getting This Right

Empty states are not an edge case. They are part of every user journey that involves dynamic content, and they appear at some of the highest-tension moments in a session: when a visitor wants something and cannot find it, when a service fails, or when a new user is trying to understand what a product does.

A blank "No results" page with no next step ends journeys that could have continued. A vague error message with no retry option loses users who would have come back in a minute. A cold, uninviting first-time dashboard turns a successful sign-up into a quick exit.

The fix is consistent across every type: treat each empty state as a small but complete design problem. Give it an illustration, a clear headline, a short description, and a single call to action. Apply the right tone for the context, check the contrast and screen reader behaviour before launch, and add it to your design system so every new feature gets the same treatment from the start.

For businesses running websites in Paphos or anywhere else, this is one of the fastest improvements available: low effort, high visible impact, and a direct line to better conversion rates on pages that already exist.

[Web Design Paphos](https://webdesignpaphos.github.io/) helps businesses in Cyprus build fast, modern websites.
