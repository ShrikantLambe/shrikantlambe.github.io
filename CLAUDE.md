# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A static single-page portfolio website hosted on GitHub Pages (`shrikantlambe.github.io`). No build process, no framework, no dependencies — pure HTML/CSS/JS. Pushing to `main` deploys automatically via GitHub Pages.

## Development Workflow

**Local preview:** Open `index.html` directly in a browser — no server needed.

**Deploy:** `git push origin main` — GitHub Pages auto-deploys on push. The site is also mirrored on Vercel (Vercel Speed Insights is loaded from `/_vercel/speed-insights/script.js`), so Vercel deployments happen automatically on push as well.

There is no build step, no `package.json`, and no CI/CD pipeline.

## Architecture

`index.html` contains all markup and a large inline `<style>` block (lines 65–351) that is a **full duplicate** of `styles.css` — not just `:root` overrides. `index.html` also has a `<link rel="stylesheet" href="styles.css">` tag (line 36), so the inline block is currently redundant with the external file; when editing any styles, update **both** to keep them from drifting apart. A JS script at the end of the body handles scroll animations, active nav tracking, project filter chips, and GA4 outbound click tracking.

- All styling uses CSS custom properties defined in `:root`; prefer editing those over hardcoding values.
- CSS Grid drives the layout; mobile breakpoint is at 720px.
- `.reveal` elements animate in via `IntersectionObserver` when they enter the viewport; the active nav link is tracked by a second `IntersectionObserver` on `section[id]` elements.
- Project filter chips (All / AI / Agents / Analytics Engineering / Data Engineering / FinTech / Product Builds) use `data-category` attributes on `.flagship` cards; selected filter persists in `localStorage`.

**`projects.json`** stores project metadata (title, GitHub link, live link, article links, tech stack). This file is not parsed at runtime — the HTML project cards are hardcoded to match it. When adding or editing a project, update both `projects.json` and the corresponding card in `index.html` to keep them in sync.

## Files

- `index.html` — main portfolio page (~1480 lines)
- `styles.css` — stylesheet linked from `index.html`, duplicated into its inline `<style>` block (see Architecture above) (~286 lines)
- `projects.json` — project metadata (source of truth for project data, not loaded at runtime)
- `sitemap.xml`, `robots.txt` — SEO assets; update `sitemap.xml` when adding new pages
- `Shrikant_Lambe_Resume.pdf` — linked from the nav CTA and hero; replace in-place to update
- `tdb_semantic_layer_series/` — self-contained sub-site ("The Data Brief" newsletter series); has its own `index.html`, `style.css`, and `article1.html`–`article6.html`. Reuses the same dark `:root` color tokens as the main site (visually consistent) but the CSS files are independent — the sub-site loads only `tdb_semantic_layer_series/style.css`, not the root `styles.css`. The index page applies a few light card overrides inline; article pages are full dark theme. Linked from the Writing section of `index.html` ("Read the Semantic Layer series") and listed in `sitemap.xml`, but has **no GA4 or Vercel Speed Insights tags** — unlike every other HTML file in the repo (see Conventions below).

## Content Sections (in order)

Section IDs match the nav links. Order in `index.html`:

1. **Hero** — intro, metrics card, contact links
2. **About** (`id="about"`) — photo + bio blurb
3. **Credentials** (`id="credentials"`) — 8 featured certs in a 3-column grid (3 rows, last row partial); "12 on LinkedIn ↗" sec-head count link goes to the full list.
4. **Projects** (`id="work"`) — 13 project cards with filter chips (All / AI / Agents / Analytics Engineering / Data Engineering / FinTech / Product Builds)
5. **Writing** (`id="writing"`) — newsletter card + article list
6. **Experience** (`id="experience"`) — timeline of job entries
7. **Stack** (`id="stack"`) — two-tier categorized tech tags (Core / Proficient)
8. **Contact** (`id="contact"`) — contact cards and role preferences
9. **Footer**

## Conventions

- Fonts: Archivo (headings and body), JetBrains Mono (code/labels) — loaded from Google Fonts.
- Color/spacing tokens are CSS custom properties in `:root` inside `styles.css`; prefer editing those over hardcoding values.
- GA4 property ID is `G-89FHE2QN8M`; the tag appears at the top of `<head>` in `index.html`. The `tdb_semantic_layer_series/` sub-site does not have it.
- Vercel Speed Insights snippet also appears immediately after the GA4 block in `index.html`, and is likewise absent from the sub-site.
