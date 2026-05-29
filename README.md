# POLARIS — The Winter Express

> A cinematic scroll-driven website for a winter steam train journey.
> Built with vanilla HTML, CSS, and JavaScript — no frameworks, no build step.

[![Made with HTML](https://img.shields.io/badge/HTML-5-E34F26?logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![Made with CSS](https://img.shields.io/badge/CSS-3-1572B6?logo=css3&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/CSS)
[![JavaScript](https://img.shields.io/badge/JavaScript-Vanilla-F7DF1E?logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](#license)

---

## ✨ Overview

POLARIS is a single-page scroll-driven website where a cinematic winter train video advances frame-by-frame as the user scrolls. Five elegantly placed text blocks fade in and out at fixed scroll positions, telling the brand story over the journey. The final CTA opens a WhatsApp conversation to reserve a seat.

The whole site lives in **one `index.html` file** (inline CSS + JS) per the project's *Scroll Animation Best Practices* guide. There is no backend, no database, no API key, and nothing to build.

---

## 🚂 Live demo

> Deploy via GitHub Pages, Netlify, Vercel, or any static host. See [Deployment](#-deployment) below.

---

## 🎬 Features

- **Scroll-scrubbed video** — `video.currentTime` is driven directly by `window.scrollY`
- **Lerp smoothing** — a `requestAnimationFrame` loop glides the playhead toward the scroll target with a soft interpolation factor (`0.08` by default), producing buttery, cinematic playback instead of rigid 1:1 scrubbing
- **Five staged text overlays** — each tied to a `data-start` / `data-end` scroll fraction, fading in with a 24px rise-in animation
- **Layered text-shadows + vignette** — three-stop shadow on headlines (`tight inner + mid halo + wide ambient`) keeps text readable over the brightest snow frames without flattening the image
- **Premium typography** — *Cormorant Garamond* for serif headlines, *Inter* for clean body
- **Transparent fixed nav** + right-edge **scroll progress rail**
- **Loader screen** that hides on `canplaythrough`, with a polling safety net and a 12-second hard fallback
- **WhatsApp CTAs** — both the top-right `Inquire` button and the bottom `Reserve your passage` CTA open WhatsApp in a new tab
- **Mobile-aware** — `playsinline` for iOS scrubbing, reduced padding/type at `< 768px`, `-webkit-overflow-scrolling: touch` for momentum

---

## 🛠️ Tech stack

| Layer | Choice | Why |
|---|---|---|
| Markup | HTML5 | Semantic, no framework overhead |
| Styling | Inline CSS3 | Single-file simplicity, no build step |
| Behavior | Vanilla JS (IIFE) | Zero dependencies, ~100 lines |
| Fonts | Google Fonts — Cormorant Garamond + Inter | Free, cinematic serif + clean sans pair |
| Video | MP4 / H.264 | Best cross-browser playback + scrubbing support |

---

## 📁 File structure

```
polaris-winter-express/
├── index.html                              # The site (HTML + CSS + JS, inline)
├── video.mp4                               # Scroll-driven video (~46 MB)
├── scroll-animation-best-practices.md      # Reference guide the project was built against
├── .env.example                            # Config template — see Configuration
├── .gitignore                              # Files git will never track
└── README.md                               # This file
```

---

## 🚀 Quick start

You only need a static file server — no `npm install`, no build.

### Option 1 — Python (no install on macOS)
```bash
git clone https://github.com/moBLAKI/polaris-winter-express.git
cd polaris-winter-express
python3 -m http.server 8000
# → open http://localhost:8000
```

### Option 2 — Node.js
```bash
npx serve .
# → open the URL it prints
```

### Option 3 — VS Code Live Server
Right-click `index.html` → *Open with Live Server*.

> **Why a server, not just opening `index.html`?**
> Browsers block `file://` requests for the video element on some setups. A local server avoids that.

---

## ⚙️ Configuration

This site has one value you must change before deploying: your **WhatsApp number**.

### 1. Set your number

Open `index.html` and search for the placeholder `YOUR_WHATSAPP_NUMBER`. It appears in **two anchors**:

```html
<!-- Top-right nav button -->
<a class="menu-btn" href="https://wa.me/YOUR_WHATSAPP_NUMBER" ...>Inquire</a>

<!-- Bottom CTA -->
<a class="cta" href="https://wa.me/YOUR_WHATSAPP_NUMBER" ...>Reserve your passage</a>
```

Replace both occurrences with your real number — international format, **no `+`, no spaces, no dashes**:

| Country | Example |
|---|---|
| Saudi Arabia 🇸🇦 | `966500000000` |
| UAE 🇦🇪 | `971501234567` |
| Egypt 🇪🇬 | `201001234567` |

### 2. (Optional) Track the value in `.env`

Copy the template:
```bash
cp .env.example .env
```
Then fill in `WHATSAPP_NUMBER=` with your real value. `.env` is in `.gitignore`, so it stays on your machine.

> ⚠️ Because this is a **static HTML site** with no build step, the `.env` file isn't read at runtime. It's a tracking aid for you and future contributors — the actual values must still be pasted into `index.html`.

---

## 🎚️ Tuning the scroll feel

Two knobs let you tune the cinematic feel without touching the structure:

| What | Where | Effect |
|---|---|---|
| **Scroll length** | `body { height: 900vh; }` | Higher = slower, more cinematic. Try `750vh` (snappier) up to `1200vh` (very leisurely). |
| **Lerp factor** | `const LERP = 0.08;` (in `<script>`) | Lower = heavier glide (`0.05`). Higher = tighter response (`0.12`). |

---

## 🎥 Replacing the video

1. Export a 1080p (1920×1080) MP4, **8–15 seconds**, H.264 codec
2. Name it `video.mp4` and drop it next to `index.html`
3. Re-test scroll feel; tune `body { height }` if the new clip's length feels off

For best scrubbing performance, keep the file under ~50 MB. Above that you'll start to feel decoding lag on slower devices.

---

## 🌐 Deployment

This is a static site — any host works. Recommended options:

### GitHub Pages
```bash
# In your repo settings → Pages → set source to `main` branch, `/ (root)`
# Site will be live at: https://<your-username>.github.io/polaris-winter-express/
```

### Netlify
- Drag the project folder onto [app.netlify.com/drop](https://app.netlify.com/drop) — done.

### Vercel
```bash
npm i -g vercel
vercel
```

### Any other host
Upload `index.html`, `video.mp4`, and any assets to the public web root.

---

## 🌐 Browser support

| Browser | Status |
|---|---|
| Chrome / Edge (Chromium) | ✅ Full |
| Safari (macOS + iOS) | ✅ Full — `playsinline` ensures iOS scrubbing |
| Firefox | ✅ Full |
| IE 11 | ❌ Not supported (drop it like it's hot) |

---

## 🔐 Security & privacy

- No backend, no cookies, no analytics, no third-party trackers out of the box
- Only network requests:
  - Google Fonts CSS + font files
  - Your WhatsApp link (only fires when clicked)
- `target="_blank"` links use `rel="noopener noreferrer"` to prevent reverse-tabnabbing and referrer leakage
- See [`.env.example`](.env.example) and [`.gitignore`](.gitignore) for the secret-handling pattern

---

## 📜 Credits

- Built by **Mohammad Tarek**
- Scroll animation pattern adapted from [`scroll-animation-best-practices.md`](scroll-animation-best-practices.md)
- Fonts via [Google Fonts](https://fonts.google.com): [Cormorant Garamond](https://fonts.google.com/specimen/Cormorant+Garamond), [Inter](https://fonts.google.com/specimen/Inter)

---

## License

MIT — do whatever you want, but please don't resell the source as-is. Attribution is appreciated.

```
Copyright (c) 2026 Mohammad Tarek

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND.
```
