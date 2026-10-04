# DESIGN.md: Governed by design

## Concept
Every claim carries its provenance, like a governed metric carries its
definition. One bold element: the interactive hero showing a question resolve
through a semantic layer. Everything else is quiet, typographic and fast.

## Audience and job
Hiring managers and recruiters for staff and principal Data & AI roles.
Job: in 30 seconds, show role, signature skill and production proof.

## Color (CSS custom properties)
--paper: #F2F4F3;   dark #121A21
--ink: #17212B;     dark #E6EBEA
--graphite: #56616C; dark #9AA6AF
--rule: #D3D9D7;    dark #2A3540
--verified: #0E7C66; dark #3DBE9C   (links, focus, governed state only)
--surface: #FFFFFF; dark #18222B    (hero panels and code blocks only)
Dark mode follows prefers-color-scheme. No gradients. No shadows except a
single 1px rule.

## Type
Schibsted Grotesk (variable) for all text. IBM Plex Mono only in code/SQL.
Scale px: 14 16 18 21 28 40 56 72. Body 18/1.55. Measure 62-68ch.
Display weight 650, tracking -0.02em. Sentence case only.
Use font-display: swap and preload the one variable file.

## Layout
12-col grid, max width 1200px, left aligned. Main = cols 1-8.
Provenance rail = cols 10-12, 14px graphite. On < 900px the rail note
sits under its paragraph.
Spacing scale: 8 16 24 40 64 104. Section gap 104 desktop / 64 mobile.
Radius 0 on text, 6px on hero panels and buttons.

## Section order
1. Hero: one-line identity, rail with role/location/availability,
   interactive question walkthrough.
2. Three case studies: Intuitive semantic layer (anonymized),
   Pipeline Sentinel, LoanLens.
3. Other builds: one line each.
4. Writing: 3 essays + Semantic Layer series.
5. Experience: role, scope, outcome.
6. Contact.

## Motion
Only the hero sequence animates on load (max 2.4s total). Everything else
responds to user action only: expand, copy, switch question.
Respect prefers-reduced-motion.

## Banned patterns
- Eyebrow labels above section headings ("about", "featured work").
- Numbered markers (01 / 02) on anything that is not a sequence.
- Strings joined with middle dots.
- Arrows or glyphs appended to link text. One external-link icon, only
  where a link leaves the site.
- Monospace for labels, stats or meta text.
- Stat strips and big-number tiles outside the case studies.
- Fake app windows or terminal mockups on every project.
- Logo walls and skills grids.
- Fade-and-slide-up on scroll for every section.
- Uniform rounded cards with soft grey shadows.
- Accenting one word in a headline with color or italics.
