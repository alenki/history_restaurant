# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Multi-page static restaurant website for "History" restaurant in Nalchik, Russia. Built with vanilla HTML/CSS/JavaScript + jQuery, bundled with Vite, deployed on Netlify. All content is in Russian.

## Commands

- `npm run dev` — Start dev server on port 7777
- `npm run build` — Production build to `dist/`
- `npm run preview` — Preview production build locally

No test framework or linter is configured.

## Architecture

### Pages

Four separate HTML pages, each as its own Vite entry point (configured in `vite.config.ts`):

| Page | File | Purpose |
|------|------|---------|
| Home | `index.html` | Main landing with slider, booking form, menu preview, testimonials, map |
| About | `about/index.html` | Restaurant history |
| Menu | `menu/index.html` | Full menu with images/prices |
| Contact | `contact/index.html` | Booking form and contact info |

Netlify routing in `netlify.toml` maps `/menu`, `/contact`, `/about` to their `index.html` files, with `/*` fallback to root.

### JavaScript

- `src/js/main.js` — Core shared logic: AOS init, navbar scroll behavior, Waypoints animations, Magnific Popup, date/time pickers, Lenis smooth scroll, GSAP parallax
- `src/js/main_page.js` — Owl Carousel setup for home page sliders
- `src/js/yandex-map.js` — Yandex Maps initialization (coordinates: 43.576, 43.4756)
- `src/js/range.js` — Price range slider for menu page

GSAP and ScrollTrigger are loaded via CDN (global `gsap`/`ScrollTrigger`), not npm. jQuery is loaded via CDN in HTML `<head>` (global `jQuery`), separate from the npm package.

### CSS

- `src/css/style.css` — Main stylesheet (~9,400 lines), contains all custom styles and CSS variables
- `src/css/about.css`, `src/css/menu.css` — Page-specific overrides
- Remaining CSS files in `src/css/` are third-party libraries (Bootstrap datepicker, AOS, Owl Carousel, Magnific Popup, icon fonts)

### Parallax System

Data-attribute driven via GSAP ScrollTrigger:
- `data-parallax-bg` — Enables background parallax on a section
- `data-parallax-speed="0.7"` — Controls parallax intensity (default 0.2)
- `data-parallax-bg-x="50%"` — Horizontal position (default center)

### Animation Classes

- `.ftco-animate` + Waypoints — Scroll-triggered fade animations (fadeInUp default, configurable via `data-animate-effect`)
- AOS attributes (`data-aos="fade-up"`) — Additional scroll animations
- `.home-slider` / `.carousel-work` — Owl Carousel instances

### Static Assets

- `public/` — Optimized WebP images, logos, testimonial photos
- `original_images/` — Unoptimized source PNGs (not deployed)
- `src/fonts/` — Custom font files (Josefin Sans)

## Key Libraries

| Library | Loaded via | Purpose |
|---------|-----------|---------|
| jQuery 3.x | CDN `<script>` | DOM manipulation, plugin host |
| Bootstrap 5.3 | npm + CDN | Grid, navbar, collapse, modals |
| GSAP + ScrollTrigger | CDN | Parallax backgrounds, scroll animations |
| Lenis | npm (ES module) | Smooth scrolling, synced with GSAP ticker |
| Owl Carousel | Local JS/CSS | Image sliders/carousels |
| AOS | npm (ES module) | Animate on Scroll |
| Magnific Popup | Local JS/CSS | Image lightbox/gallery |
| Waypoints | npm | Scroll-triggered counters and animations |
| Yandex Maps v3 | CDN | Embedded map |
