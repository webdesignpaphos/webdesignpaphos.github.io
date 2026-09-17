---
title: "How to Use Video Effectively on Your Business Website"
description: "A practical guide to using background video, explainer video, and testimonial video on your website to boost engagement and conversions."
author: "Web Design Paphos"
date: "2026-09-17"
category: "Web Design"
readTime: "8 min read"
---

Video is everywhere online, and business owners are asking the same question more than ever: should my website have video? The honest answer is: it depends entirely on the type of video and how you implement it. Done well, video on a website can increase conversions by 25 to 80 percent. Done poorly, it slows your site down, frustrates visitors on mobile, and actively hurts your chances of ranking well.

This guide covers the three main types of video used in modern web design, when each one helps, when it hurts, and exactly how to implement them without tanking your site's performance.

## The Three Types of Website Video

Most websites use video in one of three ways: as a background loop in the hero section, as an explainer video that describes what the business does, or as testimonial videos from real customers. Each serves a different purpose and carries different design and technical requirements.

Understanding the difference is the first step to making smart decisions about which to use and where.

## Hero Background Video: The Risks and Rewards

Background video loops have been fashionable in web design for years. A 10-second silent clip of your team at work, your product being made, or your location in Cyprus can create an immediate emotional impression that a static image sometimes cannot match. But hero video is also the most technically risky type.

### The LCP Problem

Largest Contentful Paint (LCP) is a Core Web Vitals metric that measures how quickly the largest visible element on your page loads. Google uses it directly in its ranking algorithm. Autoplay hero videos are the number one cause of LCP failures on visually rich websites, because video files cannot be preloaded the way images can.

If your hero video file is 8MB and the visitor is on a mobile connection, they will stare at a blank or blurry section while it downloads. Their LCP score craters. They often leave before the video even starts.

### The Correct Implementation

The solution is the **poster-frame method**. Instead of relying on the video to be your LCP element, you use a high-quality static image (the poster) as the hero's LCP element, and the video plays as a progressive enhancement once it has downloaded.

Here is what the markup looks like:

```html
<video
  autoplay
  muted
  loop
  playsinline
  preload="metadata"
  poster="hero-poster.jpg"
>
  <source src="hero-loop.mp4" type="video/mp4">
</video>
```

The `poster` attribute ensures a crisp image appears immediately. The video then fades in once it is ready. Done correctly, this approach barely affects your LCP score at all.

### File Size Rules for 2026

Keep your background video files within these limits:

- Desktop: 4MB or under
- Mobile: 2MB or under

To hit these targets, compress your video using [HandBrake](https://handbrake.fr/) (free) or [FFmpeg](https://ffmpeg.org/) (free, command-line). Use H.264 encoding at 1080p, reduce the bitrate, and aim for 24fps rather than 60fps for a looping background clip. You can also serve different files to different devices using the `media` attribute on your `<source>` elements, loading a lighter mobile cut on smaller screens.

### Accessibility: Respecting Prefers-Reduced-Motion

Some users have a system setting called `prefers-reduced-motion`, which they enable because moving content causes them discomfort or triggers vestibular disorders. Good design means pausing your video for these users. One line of CSS handles it:

```css
@media (prefers-reduced-motion: reduce) {
  video {
    display: none;
  }
}
```

When the video is hidden, your poster image takes its place automatically. You should also include a visible pause button for users who want to stop the motion manually, regardless of their system settings.

### When to Skip the Hero Video

Background video works best when you are selling something visual: a restaurant, a hotel, a spa, an architecture firm, a travel agency. If your business is more abstract (accountancy, legal services, insurance), a strong static image with clear text will almost always outperform a video loop. Forcing video into a context where it does not fit is a design mistake that costs you both performance and credibility.

## Explainer Videos: The Highest-Converting Type

If hero video is the most overused type, explainer video is the most underused. An explainer is a short, scripted video that answers one question: what do you do and who is it for?

### The Numbers

Research consistently shows that landing pages with an explainer video see conversion lifts of 20 to 80 percent. The range is wide because the quality of the video matters enormously. A well-produced 90-second explainer from a professional studio performs very differently from a shaky smartphone recording with background noise.

The recommended length for a landing page explainer is 60 to 90 seconds. Any longer and engagement drops sharply.

### Where to Place It

The most effective position for an explainer video is just below your hero section, within the first viewport scroll. Visitors who are curious but not yet convinced will scroll once. If you capture them there with a video that clearly explains what you do and who you help, they are far more likely to reach your contact form.

Do not autoplay explainer videos. Unlike background loops, these videos have sound and purpose. Give them a clear, visible play button. Use a compelling thumbnail image (not a blurry frame grabbed from the middle of the clip) to encourage people to hit play.

### Hosting Your Explainer

Never self-host your explainer video as an MP4 file on your web server. Use a video hosting service and embed it:

- **YouTube**: Free, widely trusted, but ads may appear before your video unless you pay to disable them
- **Vimeo**: Clean player, no ads on standard plans, more professional appearance
- **Wistia**: Built for business use, includes email capture, heatmaps, and A/B testing features; paid plans start at around $19 per month

The embed loads from the platform's CDN rather than your server, which keeps your page weight low. Platforms like Vimeo and Wistia also support lazy loading, meaning the video player only loads when it enters the visitor's viewport.

### Captions Are Not Optional

Every explainer video needs accurate captions. Around 85 percent of videos on Facebook are watched without sound, and a similar pattern holds on websites, particularly in offices or public spaces. Captions also make your content accessible to people who are deaf or hard of hearing, which matters both ethically and legally in the European Union. Most video hosting platforms include an auto-captioning feature. Always review the auto-generated captions for errors before publishing.

## Testimonial Videos: Building Trust at Scale

Written testimonials are easy to fake. Everyone knows it. Video testimonials are significantly harder to manufacture, which is exactly why they convert so well. When a real person looks into a camera and explains how your service helped them, the trust transfer is immediate.

### The Data Behind Video Testimonials

According to 2026 research across multiple studies:

- 79 percent of consumers are more likely to buy after watching a video testimonial
- Adding video testimonials to key pages increases conversions by an average of 25 to 34 percent
- 88 percent of marketing teams report at least a 10 percent conversion rate increase from testimonial video
- ROI exceeds 100 percent for more than half of businesses using testimonial video

The recommended length for a testimonial video is 90 to 120 seconds on sales or service pages, and 60 to 90 seconds on landing pages. The video should follow a simple structure: who the customer is and what their situation was before, what changed after working with you, and a specific outcome or result.

### Production Quality

You do not need a professional film crew for an effective testimonial video. Modern smartphone cameras are more than capable. What matters more is lighting and audio:

- Film near a window or in natural light, avoiding harsh shadows
- Use an inexpensive clip-on microphone rather than relying on your phone's built-in mic
- Keep the background clean and uncluttered

Remote testimonials recorded over video call (using tools like Loom or Riverside.fm) are widely accepted and often easier to collect from busy clients. Riverside.fm in particular records separate high-quality audio and video tracks from each participant, giving you clean footage even from remote recordings.

### Where to Place Testimonial Videos

The two highest-impact positions for testimonial videos are:

1. **Directly above or adjacent to your primary call to action** (contact form, booking button, or pricing section). This is where purchase intent is highest and where social proof converts fastest.
2. **On a dedicated testimonials or case studies page**, where longer-form videos of three to five minutes can walk through a full client story.

Avoid placing testimonial videos only on a separate page that few visitors ever reach. Bring the best one or two onto your homepage or key service pages.

## Technical Setup: Keeping Performance Intact

Regardless of which type of video you use, a few technical practices protect your site's performance.

### Lazy Load Everything You Can

Any video that is not in the first viewport should be lazy loaded. For YouTube and Vimeo embeds, a lightweight facade library called [lite-youtube-embed](https://github.com/paulirish/lite-youtube-embed) or the [Vimeo player API](https://developer.vimeo.com/player) can show a static thumbnail until the user actually clicks play, cutting the initial page weight dramatically.

For a typical YouTube embed, replacing the standard iframe with a lite version reduces the embedded page weight from around 400KB to under 10KB.

### Use Aspect Ratio Containers

One of the most common video-related layout bugs is content jumping when a video loads. The fix is a CSS aspect ratio container that reserves the correct space before the video element fills it:

```css
.video-wrapper {
  aspect-ratio: 16 / 9;
  width: 100%;
}

.video-wrapper video,
.video-wrapper iframe {
  width: 100%;
  height: 100%;
}
```

This eliminates layout shift, which also improves your Cumulative Layout Shift (CLS) score under Core Web Vitals.

### Mobile Considerations

Test every video implementation on a real mobile device over a mobile data connection, not just in your browser's device simulation mode. Key things to check:

- Does the background video actually play on iOS Safari? (It requires the `playsinline` attribute)
- Does the page layout break on a narrow screen?
- Does the autoplay video drain the user's data without their consent?

Many businesses in Paphos and across the Mediterranean serve visitors who are on tourist data packages with limited bandwidth. A video that autoloads several megabytes of data on arrival will be abandoned fast.

## Putting It All Together: A Practical Plan

If you are building or redesigning a website and want to use video well, here is a practical starting point:

1. **Homepage hero**: Consider a poster image with an optional background video overlay. If your business is primarily visual, invest in a properly compressed video file. If it is service-based, skip the video and use a strong image instead.

2. **Service or landing page**: Add a professional explainer video. 60 to 90 seconds, hosted on Vimeo or Wistia, with a thumbnail and captions.

3. **Homepage or contact page**: Place one or two testimonial videos above your primary CTA. Record them on a smartphone with good lighting and a clip-on microphone, or use Riverside.fm for remote recordings.

4. **Test your performance**: After adding any video, run your page through [PageSpeed Insights](https://pagespeed.web.dev/) and [WebPageTest](https://www.webpagetest.org/). If your LCP worsens by more than 200 milliseconds, revisit your implementation.

Video done properly is one of the most powerful tools in your web design toolkit. Video done carelessly is one of the fastest ways to lose visitors and rankings. The difference is mostly in the technical details covered above, not in the quality of the camera you used to record it.

[Web Design Paphos](https://webdesignpaphos.github.io/) helps businesses in Cyprus build fast, modern websites.
