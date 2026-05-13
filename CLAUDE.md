# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A static single-page portfolio website hosted on GitHub Pages (`shrikantlambe.github.io`). No build process, no framework, no dependencies — pure HTML/CSS/JS. Pushing to `main` deploys automatically via GitHub Pages.

## Development Workflow

**Local preview:** Open `index.html` directly in a browser — no server needed.

**Deploy:** `git push origin main` — GitHub Pages auto-deploys on push.

There is no build step, no `package.json`, and no CI/CD pipeline.

## Architecture

`index.html` contains all markup and a small inline `<style>` block (only `:root` token overrides). The bulk of shared CSS lives in `styles.css`. A JS script at the end of the body handles scroll animations, active nav tracking, project filter chips, and GA4 outbound click tracking.

- All styling uses CSS custom properties defined in `styles.css` `:root`; prefer editing those over hardcoding values.
- CSS Grid drives the layout; mobile breakpoint is at 720px.
- `.reveal` elements animate in via `IntersectionObserver` when they enter the viewport; the active nav link is tracked by a second `IntersectionObserver` on `section[id]` elements.
- Project filter chips (All / AI+ML / Data Eng / GenAI) use `data-category` attributes on `.flagship` cards; selected filter persists in `localStorage`.

**`projects.json`** stores project metadata (title, GitHub link, live link, article links, tech stack). This file is not parsed at runtime — the HTML project cards are hardcoded to match it. When adding or editing a project, update both `projects.json` and the corresponding card in `index.html` to keep them in sync.

## Files

- `index.html` — main portfolio page (~1100 lines)
- `styles.css` — shared stylesheet used by all pages (~273 lines)
- `for-apple.html`, `for-netflix.html`, `for-parafin.html` — company-targeted landing pages that link back to the main portfolio; they use `styles.css` and have their own tailored hero/content but no separate JS
- `projects.json` — project metadata (source of truth for project data, not loaded at runtime)
- `sitemap.xml`, `robots.txt` — SEO assets; update `sitemap.xml` when adding new pages
- `Shrikant_Lambe_Resume.pdf` — linked from the nav CTA and hero; replace in-place to update

## Content Sections (in order)

1. **Hero** — intro, metrics card, contact links
2. **Projects** — 8 project cards with filter chips (All / AI+ML / Data Eng / GenAI)
3. **Experience** — job entries
4. **Stack** — categorized tech tags
5. **Contact** — contact cards and role preferences
6. **Footer**

## Conventions

- Fonts: Archivo (headings and body), JetBrains Mono (code/labels) — loaded from Google Fonts.
- Color/spacing tokens are CSS custom properties in `:root` inside `styles.css`; prefer editing those over hardcoding values.
- GA4 property ID is `G-89FHE2QN8M`; the tag appears at the top of `<head>` in every HTML file.
