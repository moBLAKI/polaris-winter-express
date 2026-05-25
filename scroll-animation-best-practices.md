# Scroll-Driven Video Animation — Best Practices

A comprehensive guide for building high-performance, scroll-triggered video websites. Follow these rules to ensure smooth playback, clean code, and premium results.

---

## 1. Architecture

- Use a single HTML file with inline CSS and JS — no frameworks, no build tools
- Structure: fixed fullscreen `<video>` element + scrollable content overlay
- Total scroll height should be 5–6x the viewport height (use `body { height: 500vh; }` or similar)
- The video container is `position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; z-index: 0;`
- All text and UI sit in a `position: relative; z-index: 1;` wrapper above the video

## 2. Video Setup

- Use an MP4 file — best cross-browser support and compression
- Recommended resolution: 1920x1080 (16:9)
- Keep video duration between 8–15 seconds for best scroll feel
- The `<video>` element must have these attributes:
  ```html
  <video muted playsinline preload="auto" id="scrollVideo">
    <source src="video.mp4" type="video/mp4">
  </video>
  ```
- Never use `autoplay` or `controls` — scroll controls playback
- Add `object-fit: cover;` to the video element so it fills the viewport

## 3. Scroll-to-Video Mapping

This is the core mechanic. Scroll position maps directly to video currentTime.

```javascript
const video = document.getElementById('scrollVideo');

video.addEventListener('loadedmetadata', () => {
  const scrollHeight = document.body.scrollHeight - window.innerHeight;

  window.addEventListener('scroll', () => {
    const scrollPosition = window.scrollY;
    const scrollFraction = scrollPosition / scrollHeight;
    const videoTime = scrollFraction * video.duration;

    video.currentTime = videoTime;
  });
});
```

- Use `requestAnimationFrame` for smoother scrubbing if needed
- Always wait for `loadedmetadata` before attaching the scroll listener
- Never call `video.play()` — the scroll handler sets `currentTime` directly

## 4. Text Blocks & Scroll Triggers

Text appears and disappears based on scroll position. Each text block is tied to a specific scroll range.

### Structure

```html
<div class="text-block" data-start="0.15" data-end="0.30">
  <h2>Your headline here</h2>
</div>
```

- `data-start` and `data-end` represent scroll fractions (0 = top of page, 1 = bottom)
- Text fades in when scroll reaches `data-start`, fades out at `data-end`

### Animation

```javascript
const textBlocks = document.querySelectorAll('.text-block');

window.addEventListener('scroll', () => {
  const scrollFraction = window.scrollY / (document.body.scrollHeight - window.innerHeight);

  textBlocks.forEach(block => {
    const start = parseFloat(block.dataset.start);
    const end = parseFloat(block.dataset.end);

    if (scrollFraction >= start && scrollFraction <= end) {
      const progress = (scrollFraction - start) / (end - start);
      block.style.opacity = progress < 0.3 ? progress / 0.3 : progress > 0.7 ? (1 - progress) / 0.3 : 1;
      block.style.transform = `translateY(${(1 - Math.min(progress / 0.3, 1)) * 30}px)`;
    } else {
      block.style.opacity = 0;
    }
  });
});
```

### Positioning

- Text blocks use `position: fixed;` so they stay in the viewport while active
- Alternate text placement: left-aligned, then right-aligned, then centered
- Use generous padding from edges (8–12% from left/right)
- Never place text in the center of the frame where the main subject is — always offset

## 5. Navigation Bar

- Fixed top, transparent background, sits above everything: `z-index: 10;`
- Brand name top-left
- Menu icon top-right
- No background blur or solid color — fully transparent
- Padding: 40px from edges

## 6. Performance Optimization

- **Preload the video**: Use `preload="auto"` and consider a loading screen until the video is ready
- **Debounce scroll events**: Wrap the scroll handler in `requestAnimationFrame` to avoid jank
- **GPU acceleration**: Use `will-change: transform, opacity;` on animated text blocks
- **Avoid layout thrashing**: Only read `scrollY` once per frame, calculate everything from that single value
- **Loading indicator**: Show a simple loading screen until `canplaythrough` fires

```javascript
video.addEventListener('canplaythrough', () => {
  document.getElementById('loader').style.display = 'none';
});
```

## 7. Mobile Considerations

- Video scrubbing on iOS Safari requires the `playsinline` attribute (already included above)
- On mobile, reduce text size and increase padding
- Consider a fallback for very old devices: detect if video scrubbing works, otherwise show a static image sequence
- Test scroll feel on touch — it should feel 1:1 with finger movement
- Add `-webkit-overflow-scrolling: touch;` for smooth momentum scrolling on iOS

---

## Quick Reference — File Structure

```
project/
├── index.html      (single file — HTML, CSS, JS all inline)
├── video.mp4       (scroll-driven video, 8–15 seconds, 1080p)
└── README.md       (optional)
```

## Quick Reference — Essential CSS

```css
* { margin: 0; padding: 0; box-sizing: border-box; }
body { height: 500vh; }

#scrollVideo {
  position: fixed;
  top: 0; left: 0;
  width: 100vw; height: 100vh;
  object-fit: cover;
  z-index: 0;
}

.text-block {
  position: fixed;
  z-index: 1;
  opacity: 0;
  transition: none;
  will-change: transform, opacity;
  padding: 0 8%;
}

.text-block.left { left: 0; bottom: 15%; text-align: left; }
.text-block.right { right: 0; bottom: 15%; text-align: right; }

nav {
  position: fixed;
  top: 0; left: 0; right: 0;
  z-index: 10;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 40px;
}
```

---

*Built by Adrien Ninet — AI workflows for designers*
*Subscribe to my newsletter for more guides like this: [LINK]*
