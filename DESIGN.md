# Design brief — "Alpine engineering"

A portable design system. Paste this file (or point at it) at the start of any new
project to get a site, app, report, or slide deck that belongs to the same family
as apbenergy.github.io.

---

## 1. The idea in one paragraph

A technical document rendered by someone who cares about drawing. The page is an
engineering sheet: graph-paper ground, white document panels squared off with no
rounded corners, monospace labels for anything that is metadata, and a single
Swiss red used as a signal — never as decoration. Depth comes from one dark
"alpine night" masthead and footer that bracket the light working area. The
mountain reference is structural, not literal: contour lines, a ridgeline, survey
ticks, summit markers. Nothing is a stock photo, everything is drawn in SVG.

**Three rules that carry the identity:**

1. **Sharp corners.** `border-radius: 0` on cards, buttons, inputs, badges, tags.
   The only rounded thing on the whole site is the tool mark itself.
2. **Red is a signal, not a colour scheme.** It marks exactly one thing at a time:
   the primary action, the active nav item, a section rule tick, a drop cap, a
   summit. If red appears twice in one glance, remove one.
3. **Mono = metadata.** Every label, eyebrow, date, tag, nav item, button, and
   number uses the monospace face in uppercase with wide tracking. Prose is always
   the sans face. This split does most of the visual work.

---

## 2. Tokens

```css
:root {
  /* dark ground — masthead, footer, dark slabs */
  --night: #0e1a20;
  --night-2: #16262e;
  --night-3: #091116;

  /* light ground — the working area */
  --ground: #e0e7ea;       /* page background, carries the graph paper */
  --panel: #ffffff;        /* document sheets sitting on the ground */
  --panel-alt: #d5dee2;

  /* ink */
  --ink: #0d1418;
  --ink-soft: #55636b;     /* body prose, secondary text */
  --ink-faint: #808f97;    /* labels, metadata */
  --line: #d5dcdf;         /* interior rules */
  --line-strong: #b3bec4;  /* panel borders, section rules */

  /* cool accent — links, technical icons */
  --slate: #1d4e5c;
  --slate-deep: #123844;
  --slate-tint: #e0ebee;

  /* the signal */
  --red: #b0342c;
  --red-deep: #8c2620;     /* hover on solid red */
  --red-bright: #c9403a;   /* red on dark ground */

  /* text on the night ground */
  --on-night: #dbe4e8;
  --on-night-soft: #93a5ad;

  --max: 1160px;           /* content column */
  --measure: 68ch;         /* prose measure — never let paragraphs exceed this */
  --sans: "Inter", "Helvetica Neue", "Segoe UI", Arial, sans-serif;
  --mono: "JetBrains Mono", "IBM Plex Mono", Consolas, monospace;
  --grid: 26px;            /* graph paper pitch */
}
```

Base type: `17px`, `line-height: 1.62`, `font-feature-settings: "tnum" 1` so
numbers align in tables and specs. Headings `font-weight: 620`,
`letter-spacing: -0.022em`, `h1` line-height `1.02`.

---

## 3. The five signature elements

Reproduce these and the design reads as the same family, whatever the content.

**Graph-paper ground.** Two 1px linear gradients at `rgba(13,20,24,0.05)`,
`background-size: var(--grid) var(--grid)`, on `--ground`. Panels are opaque white
on top of it, so the grid only shows in the gutters.

**Alpine night masthead.** `--night` with a `radial-gradient(120% 90% at 78% 0%, #1a2f38, --night 62%)`,
an inline-SVG contour-line pattern at 10% white opacity drifting through the upper
right, and a three-layer ridgeline SVG pinned to the bottom edge (near range
darkest, one small red triangle as a summit beacon). The footer repeats the night
ground with an inverted ridge along its top edge, so the page opens and closes on
the same note.

**Fixed left rail** (≥980px, `46px`): white strip, vertical monospace caption
rotated 180°, a dotted "survey staff" tick scale running up it, and a red summit
triangle at the top. Push `body { margin-left: 46px }`.

**Document sheets.** `.card` = white, `1px solid var(--line-strong)`, no radius, a
`3.2rem × 2px` dark tab bleeding off the top-left corner. On hover the tab turns
red and grows to `5.4rem`, the card lifts `2px`. The tab is the detail people
remember.

**Numbered section heads.** A monospace `01` in red, the `h2` beside it, a bottom
rule in `--line-strong`, and a `2.6rem × 2px` red tick riding that rule at the left
edge. Sections are numbered in document order.

**Smaller marks worth keeping:** triangular "peak" bullets instead of discs;
`decimal-leading-zero` counters on publication and project lists; a survey-crosshair
pin and corner brackets on the hero panel; a red drop cap on the first paragraph of
the page; `::selection` in red.

---

## 4. The mark

A Swiss army knife, folded open: rounded body, one blade out, the cross on the
body. In the header it is drawn as **strokes** (`1.5` white at 82%, the cross in
`--red-bright`, rotates `-8deg` and scales on hover). As a favicon it is drawn as
**fills** on a night tile, because strokes disappear at 16px — see
[assets/favicon.svg](assets/favicon.svg).

It is also a content metaphor, not just a logo: the "what I work on" section is a
`.toolkit` — three items hinged off a shared body, each with a pivot rivet on its
left edge that fills red on hover. Reuse the metaphor whenever a project has
"several tools, one body of work".

---

## 5. Motion and states

Short and mechanical. `0.15–0.25s ease`, or
`cubic-bezier(0.2, 0.8, 0.3, 1)` for the mark. Cards lift `2px`, secondary buttons
slide `3px` right, primary buttons lift `2px`, icons lift `2px` and turn red.
Nothing fades in on scroll, nothing bounces.

Buttons: **primary** = solid red, white, uppercase mono, square, hover to
`--red-deep`. **secondary** = no box at all, just a bottom rule that turns red and
slides right. Focus is always `2px solid var(--red)` with `3px` offset.

Honour `prefers-reduced-motion: reduce` by killing all transitions.

---

## 6. Non-negotiables

- Prose paragraphs capped at `--measure`; never full-bleed body text.
- No rounded corners, no drop shadows except the one soft card-hover shadow, no
  gradients other than the two masthead radials.
- All decorative art is inline SVG in a data URI or in the markup — no image files,
  no icon fonts, no external JS.
- Every page carries the favicon, `theme-color: #0e1a20`, a real `<title>` and a
  `<meta name="description">`.
- Print stylesheet is part of the job, not an afterthought: strip the rail, nav,
  CTAs, backgrounds, and ridges; black on white; `break-inside: avoid` on entries.
- Accessibility: decorative SVG gets `aria-hidden="true" focusable="false"`; the
  red on white is only used at 2px+ weights or as a background behind white text.

---

## 7. How to adapt it to a different project

Keep the tokens, the mono/sans split, the sharp corners, and the single-signal red.
Swap the *landscape* for something native to the new subject while keeping the
same drawing language — thin strokes, low opacity, geometric, inline SVG. The
ridgeline can become a skyline, a spectrum, a load curve, a coastline; the contour
lines can become isobars, streamlines, or a wiring schematic. The rail caption and
the section numbering carry over unchanged.
