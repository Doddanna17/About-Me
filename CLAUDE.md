# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static personal portfolio site for Doddanna B (Staff Engineer). Deployed to **GitHub Pages** at [www.doddanna.com](https://www.doddanna.com).

## Architecture

- **Pure static site**: Single `index.html` (~3500 lines) with embedded `<style>` and `<script>` — no build tools, no frameworks, no bundler
- **Fonts**: Inter (body) + JetBrains Mono (tech tags) via Google Fonts
- **Analytics**: GA4 (`G-TJQ57QLSCH`) embedded in `<head>`
- **Domain**: `CNAME` → `www.doddanna.com`

## Development

No build step. Open `index.html` directly in a browser. No dependencies to install.

## Deployment

Push to `main` triggers `.github/workflows/jekyll-gh-pages.yml` which uploads the repo root as a static artifact to GitHub Pages (no Jekyll build). Single concurrent deploy; in-progress deploys are not cancelled.

## index.html Structure

The file is organized in three embedded blocks — understand this layout before editing:

### 1. `<head>` (lines ~1–90)
SEO meta tags, Open Graph, Twitter Card, JSON-LD structured data, Google Fonts, GA4, and a theme-init script that reads `localStorage('theme')` before paint to prevent flash.

### 2. `<style>` (lines ~91–2270)
All CSS in one `<style>` tag. Key sections:
- **CSS variables** in `:root` (dark theme defaults) and `[data-theme="light"]` override block
- **Light-mode overrides**: ~100 selectors under `[data-theme="light"]` (lines ~117–1420)
- **Brightness overlay**: `.brightness-control` and `.brightness-overlay` for light-mode dimming slider
- **Gamification UI**: terminal overlay, achievement panel, exploration pill, toast notifications, tech tooltips
- **Responsive breakpoints**: `@media` queries at 768px and 480px scattered throughout

### 3. `<body>` HTML + `<script>` (lines ~2270–end)
**HTML sections in order:**
1. Page loader overlay
2. Scroll progress bar
3. Custom cursor (hidden on touch)
4. Theme toggle button (sun/moon)
5. Brightness slider (light mode only)
6. **Hero** — name, title, location, CTA buttons (LinkedIn, GitHub, Email)
7. **Stats strip** — flip-counter animations (11+ years, 25+ services, 4 industries)
8. **Journey** — timeline of work experience (BitGo, BharatPe, Jumbotail, TCS)
9. **About** — bio paragraph
10. **Expertise** — cards grid (Backend, Distributed Systems, Platform, Blockchain)
11. **Industries** — icon grid
12. **Connect** — contact links
13. **Footer**
14. Gamification elements: terminal trigger, terminal overlay, exploration progress pill, achievement panel, toast container, tech tooltip

**JavaScript** (~800 lines, single IIFE):
- **Theme system**: `applyTheme()` toggles `data-theme` attribute, persists to `localStorage`, handles system preference changes
- **Brightness control**: overlay opacity slider for light mode only
- **Page loader**: name-character animation on load
- **Custom cursor**: follows mouse, glow effect, hidden on touch devices
- **Scroll effects**: progress bar, section reveal via `IntersectionObserver`, parallax on hero blobs
- **Magnetic buttons**: mousemove-based transform on `.magnetic` elements
- **Flip counter**: animated digit-flip for stats strip numbers
- **Timeline progress**: scroll-driven progress bar on journey timeline
- **Gamification**: terminal commands (`help`, `about`, `skills`, `theme`, `clear`, `links`, `achievements`, `reset`), section exploration tracking, 7 achievements, confetti, toasts
- **Tech tooltips**: hover tooltips on `.tag` elements with proficiency data from `data-*` attributes
- **Keyboard shortcuts**: backtick opens terminal, Escape closes

## Key Files

| File | Purpose |
|------|---------|
| `index.html` | Entire site (HTML + CSS + JS in one file) |
| `CNAME` / `docs/CNAME` | Custom domain config for GitHub Pages |
| `robots.txt` | Crawl rules; blocks `.git/`, `.github/`, `_includes/`, `claude-workspace/` |
| `sitemap.xml` | Single-URL sitemap for SEO |
| `favicon.svg` / `og-image.png` | Branding assets (favicon also uses GitHub avatar as fallback) |
| `.github/workflows/jekyll-gh-pages.yml` | CI/CD — static deploy to GitHub Pages |
| `_includes/head-custom.html` | Legacy Jekyll file (unused) |

## Conventions

- **Everything in one file**: all CSS and JS are embedded in `index.html` — no external `.css` or `.js` files
- **Theme system**: dark (default) and light modes via `[data-theme]` attribute on `<html>`. Dark uses `#060608` bg / `#6344d4` accent; light uses `#f8f8fa` bg / `#6344d4` accent. Always update both theme variants when changing colors.
- **CSS variables**: all colors, spacing, and typography use `:root` variables — change these rather than hardcoded values
- **Scroll reveal**: elements with class `.reveal` are animated in via `IntersectionObserver`
- **Gamification state**: stored in `localStorage` keys prefixed with `gam_` — achievements, explored sections, terminal command count
- **SEO assets**: keep `robots.txt`, `sitemap.xml`, JSON-LD in `<head>`, and Open Graph/Twitter meta tags in sync when changing site content
- When updating `sitemap.xml`, update the `<lastmod>` date
