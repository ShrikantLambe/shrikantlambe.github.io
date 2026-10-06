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
- **Nav hover lift (2026-10, research-backed):** every `.nav-right a` (the 4 section links plus the Résumé CTA) lifts `translateY(-2px)` on `:hover` using `var(--ease-spring)`, the same curve already driving every other hover-lift on the site — researched before building (not guessed): a small `-2px` for plain text links is the established pattern, distinct from the `-4px`+shadow treatment reserved for card/button surfaces, and a slight spring overshoot (not linear ease) is what makes a lift read as "confirming interactivity" rather than "just moving." Hover-only, not `.active` — a permanently-raised current-section link would look like a misaligned baseline, not a state. `.nav-links a` also gets `:active { translateY(0) scale(0.97) }`, matching this site's standing rule that every hover gets a press state too.
- **A real pre-existing bug, found while adding this:** `.nav-cta`'s own `transition`/`:active` had been silently dead code — `.nav-right a` (class+type, specificity 0,1,1) is actually *more specific* than `.nav-cta` alone (class only, 0,1,0), so `.nav-cta`'s declarations were always losing to `.nav-right a`'s plain `transition: color .18s`. Confirmed via computed style before fixing. Fixed by renaming every `.nav-cta` selector to `a.nav-cta` (class+type, same 0,1,1 specificity as `.nav-right a`, and later in source order — wins correctly now). Keep this in mind for any future `.nav-right` child that needs its own transition/hover/active behavior: give it a type selector, not just a class, or it'll lose silently the same way.
- `@media (prefers-reduced-motion: reduce)` disables the reveal transition/transform and both pulse animations outright (styles.css, right after `.reveal.in`). It does **not** disable `:active` press feedback or the hover spotlight — both are discrete, user-triggered, non-auto-playing, and not the kind of motion that reduced-motion targets. Any new *auto-playing or continuous* animation added to the site still needs a corresponding entry in that block.

### Full-page section snap (2026-10, explicit user request to mimic a FullPage.js-style site)

This is a bigger exception than anything else in this file — it changes the page's core *interaction model*, not just a visual detail, and was added only after flagging the tradeoff (it fights skimmability on a content-dense CV site) and getting an explicit "do it anyway." Know this exists before touching scroll behavior, nav, or section markup.

- **Desktop (wheel + keyboard):** hand-rolled vanilla JS (no library — this repo has zero dependencies by design) hard-snaps between stops at the end of `index.html`'s script block, right after the reveal-animation setup. A `wheel`/`keydown` handler only `preventDefault()`s and snaps when the viewport is already at the *current* stop's top/bottom edge; otherwise it does nothing and native scrolling proceeds untouched. Stops shorter than the viewport (`#about`, `#credentials`, `#stack`) get a single stop (arrival already satisfies "at bottom") rather than a dwell-then-scroll; taller ones (`#writing`, `#experience`) keep scrolling natively inside until their own edge is reached. A `snapping` lock (850ms) blocks re-entry while a snap's `scrollIntoView({behavior:'smooth'})` is in flight.
- **The stop list isn't static:** `snapSections` is rebuilt by `computeSnapSections()` — `.hero`/`#about`/`#credentials`, then every currently-visible `.flagship` (each of the 13 projects is its own stop — see Work section below), then `#writing`/`#experience`/`#stack`/`#contact`. This is a top-level `let` + function, not a `const` inside the reduced-motion block, specifically so `applyFilter()` (which hides/shows `.flagship` elements) can call it again after a filter chip changes, regardless of whether the hijack itself is active. `history.scrollRestoration = 'manual'` is set alongside it — with ~20 stops instead of 8, a browser restoring a raw scroll position on reload/back-nav is a bigger deal than it used to be.
- **Mobile/touch:** deliberately gets no JS hijack at all — real touchscreens never fire `wheel` events, so the handler is naturally a no-op there. Instead, `@media (max-width: 720px)` applies native `scroll-snap-type: y proximity` (not `mandatory`) with `scroll-snap-align: start` on the 8 major sections (`.flagship` is deliberately **not** in this CSS list — mobile keeps `#work` as one soft stop, not 13) — a softer, battle-tested equivalent instead of hand-rolled touch-gesture math, which is a common source of janky, unreliable scroll-hijack implementations.
- **Accessibility:** the entire JS hijack is skipped outright under `prefers-reduced-motion: reduce` (checked once at setup) — scroll-hijacking is exactly the pattern that preference exists to opt out of. The CSS mobile fallback is also disabled under reduced-motion (`scroll-snap-type: none`). The keydown handler ignores `ArrowDown`/`ArrowUp`/`PageDown`/`PageUp` whenever focus is on an input/textarea/select/contenteditable element.
- **Known, accepted limitation:** the last stop (`#contact`) can't always be scrolled exactly to the viewport top if there isn't enough page content below it (the footer is short) — the browser clamps to its natural max scroll position instead. This is correct, expected browser behavior, not a bug to fix.
- If you add a 9th *major* section, add it to `computeSnapSections()` (JS) **and** the two CSS selector lists (`styles.css` and the inline block) in the same edit — all three must stay in sync or desktop/mobile will disagree about where sections snap. Adding/removing a *project* needs no such sync — `computeSnapSections()` already re-queries `.flagship` live.

### Work section: cinematic per-project snap stops (2026-10, third attempt — see history below)

Explicit follow-up request, revised twice. Each one taught something that shaped this version:
1. **Continuous 1:1 wheel-to-`scrollLeft` tracking** (a native-feeling horizontal scrollbar) — rejected as "not impressive and confusing": it mixed two different interaction models on one page (discrete snap between sections, continuous drag within Work).
2. **A discrete horizontal slide-carousel** — fixed the "confusing" part (one gesture, one move, consistent with the rest of the site), but after seeing an Apple-product-page-style reference, the user clarified they never wanted horizontal movement at all. What they actually wanted from that reference: bigger/bolder cinematic per-project visual treatment, and a "hero pin-and-scale" effect — while keeping the "one thing fills the whole screen at a time" shape, which had been right both times.
3. **This version:** no horizontal movement, no separate nested mechanism at all. Each `.flagship` is simply its own stop in the *same generic vertical snap system* used for every other section (see "Full-page section snap" above — read that first). The only genuinely new things are the visual redesign and a scroll-linked scale effect.

- **Structure:** no wrapper divs, no special-cased JS. `computeSnapSections()` just includes every visible `.flagship` in the same `snapSections` array as `.hero`/`#about`/etc. The existing generic `atTop()`/`atBottom()`/`goTo()` needed zero changes — they already work off any element's `offsetTop`/`offsetHeight`.
- **Sizing — `min-height`, not `height`:** inside `@media (min-width: 961px) and (prefers-reduced-motion: no-preference)`, `.flagship` gets `min-height: calc(100vh - 58px)`. A card whose content runs long simply pushes the page taller, exactly like `#writing`/`#experience` already do in the generic system — there is no internal-overflow scrolling, no "let the hovered panel finish scrolling before advancing" logic, because `min-height` (not a hard `height`) never creates that problem in the first place. The previous two attempts both needed extra machinery to solve overflow; this one has none to solve. Below 961px, or under reduced motion, `.flagship` is completely untouched — today's normal card, normal vertical stack, not a fallback path that needs its own testing.
- **Title is a full-width header row, not part of the two-column content — a follow-up fix, not part of the original plan:** the first cinematic version kept the eyebrow+title *inside* the text column (`.flag-meta`) at `clamp(40px,5.5vw,68px)`, which routinely made that column far taller than `.flag-preview` — even with `align-items: start`, the two columns still read as visually uneven, just no longer floating-in-empty-space uneven. Fixed by pulling `.flag-num`/`.flag-tag`/`h3` out into a sibling wrapper, `.flag-head`, and giving `.flagship` a third named grid area for it: `grid-template-areas: "head head" "meta preview"` (`.flagship.rev`: `"head head" "preview meta"`). The title is smaller now too — `clamp(32px,4vw,52px)`, down from 68px — because a full-width row doesn't need the extra size to read as bold that a narrow single-column headline did, and it now wraps to far fewer lines. `.flag-meta`/`.flag-preview` are correspondingly more balanced in height on their own (title was the single biggest contributor to the mismatch), though `align-items: start` is still kept for whatever difference remains.
- **This DOM change reaches the base (non-cinematic) rules too, not just the cinematic gate:** `.flag-head`/`.flag-meta`/`.flag-preview` are a structural change to every `.flagship` article (13×, mechanical), so the *base* `.flagship` rule and the `max-width: 920px` mobile collapse both needed their own `grid-template-areas` ("head preview"/"meta preview" base; "head"/"meta"/"preview" stacked on mobile) to keep rendering correctly outside the cinematic gate — this is the one part of the feature that was never going to be purely additive inside the `>= 961px` media query. `.flag-num`/`.flag-tag`/`h3`'s CSS selectors moved from `.flag-meta X` to `.flag-head X` accordingly. Base/mobile/reduced-motion rendering is verified equivalent to before, not byte-identical (the mechanism changed from implicit 2-item grid placement to explicit named areas) — check visually if touching this again, not just by diffing computed pixel values.
- **Source order matters here — a gotcha hit once already:** this whole block must come *after* the base `.flagship`/`.flag-meta`/`.flag-preview` rules in both `styles.css` and the inline block, not before. It overrides several of the same properties (`grid-template-columns`, font-size, etc.) at the *same* specificity as the base rules (same selectors), and CSS resolves equal-specificity conflicts by source order, not by "wrapped in a media query" — a media-query block placed earlier in the file still loses to a later plain rule for any property they share. This silently broke the font-size/grid-ratio overrides (while unrelated properties like `min-height`, which the base rule never sets, worked fine) until the block was moved to after the Flagship Projects section.
- **Pin-and-scale, reusing the existing pattern:** rather than inventing scroll-math, this reuses the same native `animation-timeline: view()` technique already used for `.reveal` and the kinetic headings. `.flag-preview` scales from `0.82` to `1` across `animation-range: entry 0% entry 90%` — because each card is ~viewport height, "entry" naturally spans close to one full snap transition, same reasoning as the kinetic-heading ranges, no manual tuning needed. Fallback for browsers without `animation-timeline` reuses the exact `.reveal`/`.reveal.in` shape: a base `scale(0.82)` + `transition`, an `IntersectionObserver` (same file, right after the `.reveal` one) adds `.in` → `scale(1)`.
- **Two transform-collision fixes — the recurring lesson of this feature:** both prior attempts broke at runtime because something *else* was also animating/transitioning the `transform` property on the same element, and in CSS, **animations win over inline styles regardless of specificity**, while two transitioning rules at equal specificity just fight. Fixed here by (a) disabling `.flag-preview`'s existing hover-lift (`translateY(-4px)` from the motion-polish pass) inside this same gate, since `.flag-preview` now owns `transform` via the scale animation instead, and (b) moving the `.reveal` class off `article.flagship` and onto `.flag-meta` only, so the text column keeps its own subtle fade/settle while `.flag-preview` exclusively owns the dramatic scale. **If project cards ever stop animating correctly again, check for a second rule touching `transform` on the same element before anything else** — this exact bug class has now caused three separate runtime failures across two prior attempts.
- Re-entering Work by scrolling back up from `#writing` lands on whichever project was last at the bottom edge — normal generic-system behavior, nothing project-specific to it anymore.

## Content voice

This is the site's actual strength — keep it. Copy is fact-dense (specific numbers: "94% median confidence," "11s time-to-resolution," "$0 infra cost") rather than adjective-dense ("seamless," "cutting-edge," "revolutionize" — a scan of the whole page found essentially none of these). **Hold this line** when writing new project copy: every claim should be a number or a named, checkable fact, not a superlative.

**Emoji discipline:** exactly one emoji per project card, in its `.outcome` line, as a visual anchor — currently 13 outcome badges, one per project (🔍☕⚡📊🤖🏗🔄🎯🚀🎓📚🗓). Preview mock-UI content (chat exchanges, memo headers, status titles) uses the site's existing plain-glyph vocabulary instead: `✓` for done/success states (`.ttl`, `.af-step.done`), `◆` for AI/system-response markers (`.ch-ai::before` already injects this automatically — never add a redundant emoji to `.ch-ai` text), `●`/`○` for running/pending. The one exception is `⚠` in `.anomaly-badge` (Growth Agent preview) — kept because it's paired with `--red` styling and functions as a real severity marker, not decoration; don't add more like it without the same red/status pairing to justify it.

## Breakpoints

Single breakpoint: `max-width: 720px` (used 6× across styles.css). No tablet-specific tuning — the fluid `clamp()` type scale and CSS Grid auto-wrap absorb most of the range between 720px and desktop without a second breakpoint.

Below 720px, the in-page nav links (`#work`/`#writing`/`#experience`/`#contact`) collapse into `.nav-links`, a hamburger-toggled dropdown (`.nav-toggle` button, `position:absolute` panel anchored to `nav.top`, `max-height` transition, closes on link click). The résumé CTA stays inline in the bar at all times, outside the collapsible menu — it's the one nav item that should always be reachable in a single tap.

## Interaction states

Anything with mutually-exclusive selection state (currently: project filter chips) sets `aria-pressed="true"/"false"` on the `<button>` alongside the `.active` class — update both together, never just the visual class. The mobile nav toggle mirrors this pattern with `aria-expanded` on `.nav-toggle`.
