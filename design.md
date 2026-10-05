# Design System

Documents the design system **as it actually exists** in `styles.css` / `index.html` today, so future edits stay consistent instead of drifting. This is a description of current reality, not an aspirational spec — where something below is a recommendation rather than existing practice, it's marked "Recommended."

## Identity

Dark, mono-accented, data-dense. Reads as an engineer's tool, not a marketing site — deliberately restrained (near-black + single mint accent, no stock icon sets). Keep it that way: resist adding a second accent color or a hero illustration.

**2026-10 motion/surface pass (user-requested "modern look + more animation"):** the restraint above governs *color and layout*, not motion — that axis was deliberately expanded. The site now has two very low-opacity (6–8%), single-accent-only ambient radial glows (hero, contact — the two flattest sections) plus cursor-reactive spotlight glow on `.cred-card`/`.flag-preview`, spring-eased hover lifts, and press/active feedback across interactive elements (see Motion below). This was a scoped, explicit exception, not drift: still one accent color, still no stock gradients-as-wallpaper, still nothing that reads as decoration unconnected to an interactive or depth purpose. Don't treat this as license to keep adding — the bar for a new glow/gradient is still "does this serve legibility or interaction feedback," not "would this look cool."

## Color

All colors are CSS custom properties in `:root` (styles.css:1–9, duplicated in `index.html`'s inline block — edit both).

| Token | Value | Use |
|---|---|---|
| `--bg` | `#08080a` | Page background |
| `--bg-1` | `#0f0f12` | Card/panel background (one step up) |
| `--bg-2` | `#16161a` | Hover state for cards/panels |
| `--bg-3` | `#1e1e23` | Rarely used, deepest panel tier |
| `--fg` | `#fafafa` | Primary text, headings |
| `--fg-1` | `#c4c4cc` | Body copy |
| `--fg-2` | `#8b8b95` | Secondary text (labels, meta) |
| `--fg-3` | `#7d7d8e` | Tertiary text (timestamps, footer, micro-labels) |
| `--line`, `--line-1`, `--line-2` | `#22222a` / `#2f2f38` / `#3a3a45` | Borders, from subtle to visible |
| `--accent` / `--accent-bright` / `--accent-dim` | `#10d47f` / `#34eaa0` / `#0a7f4d` | The one accent color — CTAs, links, highlights, active states |
| `--red` / `--amber` / `--blue` | `#f87171` / `#fbbf24` / `#60a5fa` | Status only (error / warning / info) — not decorative |

`--fg-3` was originally `#5a5a66` (2.94:1 on `--bg`, 2.81:1 on `--bg-1` — both fail WCAG AA's 4.5:1 minimum) and was lightened to the current value (~4.9:1 on both backgrounds, computed not eyeballed). Don't darken it back below ~4.5:1 — it's used for real content (footer copy/links, credential "Verify ↗" links, timeline locations, preview-card micro-labels), not just decoration.

Never hardcode a hex value or a raw `rgba(r,g,b,a)` triple that duplicates a token — use `var(--accent)` etc., and for alpha variants use `rgba(var(--accent-rgb),X)` / `rgba(var(--amber-rgb),X)`.

## Typography

Two fonts, used with intent — don't introduce a third.

- **Archivo** (`var(--sans)`) — all headings and body copy.
- **JetBrains Mono** (`var(--mono)`) — anything that reads as data, UI chrome, or metadata: nav links, buttons, tags, timestamps, section labels, stat numbers, code/preview content. If it's a *label* or a *number*, it's mono. If it's *prose*, it's Archivo.

Heading scale (all fluid via `clamp()`, mobile-min → viewport-scaled → desktop-max):

| Element | clamp() |
|---|---|
| `h1.headline` | `clamp(42px, 7.5vw, 96px)`, weight 500, tracking -0.04em |
| `.about-heading` (h2-equivalent) | `clamp(38px, 5.5vw, 70px)` |
| `.sec-head h2` | `clamp(30px, 4vw, 50px)`, weight 500, tracking -0.035em |
| Card h3 | `clamp(26px, 3.2vw, 40px)` |
| Sub-heading | `clamp(22px, 2.8vw, 36px)` |
| `.hero-sub` | `clamp(15px, 1.4vw, 18px)` |

Body/UI sizes cluster tightly: **10–13px** for mono labels/meta (10.5px and 11.5px are both in real use — not a typo, just two adjacent micro-sizes for different densities), 13–16px for body copy, 18–28px for stat numbers and pull-quotes.

Weights in use: 300 (headline slash only), 400 (body default), 500 (most headings/buttons — the workhorse weight), 600 (emphasis, `<strong>`), 700 (rare, brand mark only). Don't reach for 700 casually — 600 is the ceiling for "emphasis" almost everywhere on the site.

Letter-spacing: headings and buttons run tight (-0.01em to -0.04em, tighter at larger sizes); uppercase mono labels run loose (+0.06em to +0.12em). Never leave letter-spacing at browser default on a heading.

## Spacing

Not a strict 4px/8px grid — values are chosen per-component (6, 10, 14, 18, 22, 26px etc. all appear deliberately, not just multiples of 8). **Recommended discipline going forward:** when adding a new component, reuse the closest existing value from a similar component rather than inventing a new one — the current spacing feels consistent because of *proximity*, not a rigid scale. Section rhythm is the one place with a real, deliberate pattern:

- `.sec-head` top padding: 100px (only at first sec-head after hero; `.no-border` variant removes it)
- Section-to-section gap: `margin-top: 40px` on `.sec-head`
- Mobile side gutter: 20px (`.wrap` padding drops from 32px to 20px at 720px)

## Components

- **Buttons** (`.btn`): 12px/18px padding, 6px radius, mono font. `.primary` = solid accent fill; default = outlined/bg-1. Hover: border/bg lighten one step + `translateY(-1px)` (spring-eased, see Motion); `.primary` additionally gets a soft accent-tinted glow shadow on hover. Press (`:active`): `scale(0.97)`.
- **Cards** (`.cred-card`, `.flag-preview`, `.nl-card`): `--bg-1` fill, 1px `--line` border, 6–12px radius (never higher — no bubble/pill cards). Two different hover treatments, by design, not an inconsistency to fix: `.cred-card` → border `--accent`, bg `--bg-2`, `translateY(-1px)`; `.flag-preview` → border `--line-2`, `translateY(-4px)`, two-layer box-shadow (ambient + a 1px accent ring that fades in) — both its rest and hover states declare the same shadow-layer count so the transition actually eases instead of snapping. Both also get a cursor-reactive spotlight: a `::before`/`::after` radial-gradient overlay positioned at `--mx`/`--my` custom properties (updated by a `mousemove` listener, gated behind `(hover: hover)` so touch devices skip it entirely), accent-tinted at 8–14% opacity, fading in on hover. Both also get a `:active` press-scale (0.97–0.98). `.nl-card` is a static sticky info panel, not a clickable unit — it has no hover state and no spotlight.
- **Tags/pills** (`.tag`, `.tier-pill`, `.pill`): mono, small, `--bg-1` fill, 4px radius — flat, no shadow, no gradient.
- **Chips** (filter buttons): same tag styling but interactive; `.active` = solid accent fill (the *only* place a pill goes solid-filled outside of `.btn.primary`).
- Border-radius scale in practice: 3–4px (tags, small chrome) / 6px (cards, buttons) / 8–12px (large panels only: `.flag-preview`, `.nl-card`). Don't go above 12px anywhere — that's the ceiling that keeps the site feeling like tooling, not a consumer app.

## Motion

- Reveal-on-scroll: `.reveal` fades + translates 18px **+ scales from 0.985** on `IntersectionObserver` (or the native `animation-timeline: view()` path), staggered 60ms per element, 0.6s ease. This is the main "designed" entrance — everything else is a fast utility transition.
- Hover/interactive transitions: 0.18–0.2s for color/background/border/shadow, no exceptions there. Transform-only hover/press feedback (button and card lifts) uses `var(--ease-spring)` (`cubic-bezier(0.34, 1.56, 0.64, 1)`, a slight overshoot) at 0.2–0.25s instead of linear `ease` — this is the one deliberate exception to "no exceptions," scoped to transforms only so color/shadow still settle smoothly without overshoot artifacts. Don't apply the spring curve to anything that isn't a transform.
- Press/active feedback (`:active { transform: scale(...) }`, 0.95–0.98 depending on element size) is present wherever hover feedback is: `.btn`, `.nav-cta`, `.chip`, `.cred-card`, `.flag-preview`. Add it to any new interactive element in the same family — its absence was a real gap the 2026-10 pass closed.
- Cursor-reactive spotlight glow on `.cred-card`/`.flag-preview` (see Components) — the one new ambient/decorative-adjacent effect, justified because it's interaction-bound (only visible on hover, tracks the actual cursor) rather than auto-playing.
- Two `@keyframes` pulses exist (`--accent` dot, `--amber` warning icon) — both are status indicators, not decoration. Don't add a third pulse animation for something that isn't communicating live/active state.
- Nav links (`.nav-links a`, desktop only — `min-width: 721px`) get an animated underline (`::after`, `scaleX` 0→1) on hover/`.active`. Deliberately excluded from the mobile dropdown, which already uses row separators as its own affordance.
- `@media (prefers-reduced-motion: reduce)` disables the reveal transition/transform and both pulse animations outright (styles.css, right after `.reveal.in`). It does **not** disable `:active` press feedback or the hover spotlight — both are discrete, user-triggered, non-auto-playing, and not the kind of motion that reduced-motion targets. Any new *auto-playing or continuous* animation added to the site still needs a corresponding entry in that block.

## Content voice

This is the site's actual strength — keep it. Copy is fact-dense (specific numbers: "94% median confidence," "11s time-to-resolution," "$0 infra cost") rather than adjective-dense ("seamless," "cutting-edge," "revolutionize" — a scan of the whole page found essentially none of these). **Hold this line** when writing new project copy: every claim should be a number or a named, checkable fact, not a superlative.

**Emoji discipline:** exactly one emoji per project card, in its `.outcome` line, as a visual anchor — currently 13 outcome badges, one per project (🔍☕⚡📊🤖🏗🔄🎯🚀🎓📚🗓). Preview mock-UI content (chat exchanges, memo headers, status titles) uses the site's existing plain-glyph vocabulary instead: `✓` for done/success states (`.ttl`, `.af-step.done`), `◆` for AI/system-response markers (`.ch-ai::before` already injects this automatically — never add a redundant emoji to `.ch-ai` text), `●`/`○` for running/pending. The one exception is `⚠` in `.anomaly-badge` (Growth Agent preview) — kept because it's paired with `--red` styling and functions as a real severity marker, not decoration; don't add more like it without the same red/status pairing to justify it.

## Breakpoints

Single breakpoint: `max-width: 720px` (used 6× across styles.css). No tablet-specific tuning — the fluid `clamp()` type scale and CSS Grid auto-wrap absorb most of the range between 720px and desktop without a second breakpoint.

Below 720px, the in-page nav links (`#work`/`#writing`/`#experience`/`#contact`) collapse into `.nav-links`, a hamburger-toggled dropdown (`.nav-toggle` button, `position:absolute` panel anchored to `nav.top`, `max-height` transition, closes on link click). The résumé CTA stays inline in the bar at all times, outside the collapsible menu — it's the one nav item that should always be reachable in a single tap.

## Interaction states

Anything with mutually-exclusive selection state (currently: project filter chips) sets `aria-pressed="true"/"false"` on the `<button>` alongside the `.active` class — update both together, never just the visual class. The mobile nav toggle mirrors this pattern with `aria-expanded` on `.nav-toggle`.
