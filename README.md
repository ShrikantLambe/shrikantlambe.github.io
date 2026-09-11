# Shrikant Lambe — Technical Portfolio

Source for [shrikantlambe.github.io](https://shrikantlambe.github.io), a single-page portfolio site showcasing data & AI engineering projects, work experience, and technical stack. Static HTML/CSS/JS — no build step, no framework, no dependencies.

## Structure

- `index.html` — the full portfolio page: hero, 13 project cards (filterable by AI / Agents, Analytics Engineering, Data Engineering, FinTech, Product Builds), credentials, experience, tech stack, and contact sections
- `styles.css` — stylesheet linked from `index.html`, duplicated into its inline `<style>` block for historical reasons (see `CLAUDE.md`)
- `projects.json` — source-of-truth metadata for each project (GitHub link, live demo, article links, tech stack) — kept in sync with the hardcoded cards in `index.html`
- `Shrikant_Lambe_Resume.pdf` — linked from the nav and hero; replace in place to update
- `sitemap.xml`, `robots.txt` — SEO

## Local preview

Open `index.html` directly in a browser — no server required.

## Deploy

```bash
git push origin main
```
GitHub Pages auto-deploys on push to `main`.

## Updating a project

Edit both `projects.json` and the matching card in `index.html` — the JSON isn't parsed at runtime, so the two need to stay in sync manually.

---
See `CLAUDE.md` for detailed conventions (design tokens, animation behavior, GA4 tracking) if editing with an AI coding assistant.
