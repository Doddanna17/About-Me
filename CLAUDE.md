# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static personal portfolio site for Doddanna B (Staff Engineer). Deployed to **GitHub Pages** at [www.doddanna.com](https://www.doddanna.com).

## Architecture

- **Pure static site**: Single `index.html` with embedded CSS and vanilla JS — no build tools, no frameworks, no static site generator
- **Fonts**: Inter (body) + JetBrains Mono (tech tags) loaded from Google Fonts
- **Analytics**: GA4 (`G-TJQ57QLSCH`) embedded directly in `index.html`
- **Domain**: Configured via `CNAME` — points to `www.doddanna.com`
- **Design**: Dark theme (#0a0a0b) with violet accent (#6e56cf), scroll-reveal animations, responsive single-page layout

## Development

No build step required. Open `index.html` directly in a browser to preview.

## Deployment

Automated via `.github/workflows/jekyll-gh-pages.yml`:
- **Trigger**: Push to `main` branch or manual dispatch
- **Pipeline**: Checkout → Upload static files → Deploy to GitHub Pages (no Jekyll build)
- Single concurrent deployment allowed; in-progress deploys are not cancelled

## Key Files

| File | Purpose |
|------|---------|
| `index.html` | The entire portfolio site (HTML + CSS + JS) |
| `README.md` | Repository documentation |
| `CNAME` / `docs/CNAME` | Custom domain configuration |
| `.github/workflows/jekyll-gh-pages.yml` | CI/CD pipeline (static deploy) |

## Conventions

- All site content lives in `index.html`
- No external CSS or JS files — everything is embedded for simplicity and performance
- `_includes/head-custom.html` is a legacy Jekyll file (unused, GA4 now lives in `index.html`)
- To change content, edit `index.html` directly
- Five sections: Hero, About, Expertise, Connect, Footer
