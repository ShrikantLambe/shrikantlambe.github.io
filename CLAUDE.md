# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A static single-page portfolio website hosted on GitHub Pages (`shrikantlambe.github.io`). No build process, no framework, no dependencies — pure HTML/CSS/JS. Pushing to `main` deploys automatically via GitHub Pages.

## Development Workflow

**Local preview:** Open `index.html` directly in a browser — no server needed.

**Deploy:** `git push origin main` — GitHub Pages auto-deploys on push.

There is no build step, no `package.json`, and no CI/CD pipeline.

## Architecture

Everything lives in a single file: `index.html` contains all markup, all CSS (inline `<style>` block), and a minimal JS script at the end of the body.

- All styling uses CSS custom properties defined at the top of the `<style>` block.
- CSS Grid drives the layout; a single media query at 1024px handles mobile.
- The only JavaScript is an `IntersectionObserver` that stagger-animates elements with class `.reveal` on scroll (about 6 lines of code).
- Project demo previews are static HTML/CSS mockups — they are not live iframes.

**`projects.json`** stores project metadata (title, GitHub link, live link, article links, tech stack). This file is not parsed at runtime — the HTML project cards are hardcoded to match it. When adding or editing a project, update both `projects.json` and the corresponding card in `index.html` to keep them in sync.

## Content Sections (in order)

1. **Hero** — intro, metrics card, contact links
2. **Projects** — 6 project cards with inline demo previews
3. **Experience** — 6 job entries
4. **Stack** — categorized tech tags
5. **Contact** — contact cards and role preferences
6. **Footer**

## Conventions

- Fonts: Fraunces (serif headings), Outfit (body), JetBrains Mono (code/labels) — loaded from Google Fonts.
- Color/spacing tokens are CSS custom properties in `:root`; prefer editing those over hardcoding values.
- `index (1).html` is a legacy backup and should be ignored.
