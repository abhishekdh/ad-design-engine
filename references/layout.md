# Layout

**Read at:** phase 1 (concept), phase 3 (building).

---

## Structure is information

Every structural device - a border, a divider, a number, a label, an eyebrow, a card - either encodes
something about the content or decorates it. Decoration is the default failure mode, and it is
recognizable.

Before adding any structural device, answer what it tells the reader that position and spacing do
not. If there is no answer, the device is decoration.

The clearest case: **numbered markers (01 / 02 / 03) are only correct when the content is actually a
sequence.** A stepped process, a timeline, a ranking. Three parallel features are not a sequence, and
numbering them tells the reader something false.

---

## Spacing is the primary tool

Space communicates relationship before any border does.

```
tight   4-8px     "these are one thing"      icon + label, value + unit, label + input
close   12-16px   "same group"               list items, form fields, card contents
group   24-32px   "related but separate"     card to card, subsection to subsection
break   48-64px   "new group"                section to section
context 96-160px  "new context"              hero to content, major division
```

**If a divider line seems necessary, the spacing is usually wrong.** A divider is a symptom of
insufficient spacing contrast. It is legitimate in structurally identical dense lists (table rows,
settings rows) where space would waste too much room.

Container escalation ladder - use the lightest tool that works:

1. Space alone
2. A single divider
3. A subtle border
4. A background change (a card)

Each step adds visual weight. Never box the most important element on a screen; let it sit directly
on the background.

---

## One scale, no exceptions

```css
:root {
  --space-1: 0.25rem;  --space-2: 0.5rem;   --space-3: 0.75rem;
  --space-4: 1rem;     --space-6: 1.5rem;   --space-8: 2rem;
  --space-12: 3rem;    --space-16: 4rem;    --space-24: 6rem;
}
```

Every margin, padding, and gap comes from the scale. A `padding: 13px` anywhere means someone
nudged a value until it looked right instead of fixing the reason it looked wrong. It is also the
single clearest signal in a codebase that no system is in force.

---

## Grid over arithmetic

```css
/* Correct */
.row { display: grid; grid-template-columns: 2fr 1fr; gap: var(--space-6); }

/* Wrong - the gap is now part of the width calculation, and it breaks at every breakpoint */
.row > * { width: calc(66.66% - 12px); }
```

Use `flex` for one-dimensional runs of content whose sizes come from the content. Use `grid`
whenever positions are being defined.

`subgrid` solves the alignment problem that used to require fixed heights - it lets a child
participate in the parent's tracks, so rows across sibling cards line up even with different content
lengths:

```css
.cards { display: grid; grid-template-rows: auto 1fr auto; }
.card  { display: grid; grid-row: span 3; grid-template-rows: subgrid; }
/* every card's title, body, and footer now align across the row */
```

---

## Container queries, not media queries

A component's layout depends on **how much room the component has**, not on how wide the window is.
The same card in a sidebar and in a main column wants different layouts at the same viewport width.

```css
.card-region { container-type: inline-size; }

@container (min-width: 30rem) {
  .card { grid-template-columns: 8rem 1fr; }
}
```

Use `inline-size` in nearly all cases; it only contains the inline axis, so block height still grows
with content. `container-type: size` contains both axes and requires an explicit height, which
breaks most layouts.

Container units resolve against the nearest container: `cqi` (inline size) is the useful one;
`cqb`, `cqw`, `cqh`, `cqmin`, `cqmax` also exist.

```css
.card h3 { font-size: clamp(1rem, 0.875rem + 1cqi, 1.5rem); }
```

Note the `rem` term - see `type.md` for why a bare `cqi` font-size is an accessibility failure.

**Media queries remain correct for genuinely page-level changes:** navigation collapsing, sidebar
appearing, page-level column count.

**Pitfall:** `container-type` establishes containment. An element cannot be its own container, so
the query goes on the wrapper and the styles go on the child.

Breakpoints, when a media query is right: `640 / 768 / 1024 / 1280 / 1536`. Test at
**320px**, 768, 1024, 1440. 320 is the real floor and the one that breaks.

---

## Page frame

```css
.shell { max-width: 87.5rem; margin-inline: auto; padding-inline: var(--space-6); }
```

Use `min-height: 100dvh`, never `100vh`. On mobile browsers `vh` is computed against the viewport
*without* the retracting chrome, so a `100vh` hero is taller than the visible screen and its bottom
content is cut off on load. `dvh` tracks the actual visible area.

---

## Hero

Hard rules, both modes:

- **It fits the initial viewport.** Headline plus support plus the primary action, visible without
  scrolling, at 1440x900 and on a 375-wide phone.
- **Headline at most 2 lines. Support text at most about 20 words.**
- **Top padding capped.** Large top padding pushes the actual content below the fold to buy
  emptiness that could come from tighter internal spacing.
- **At most 4 text elements total.** Eyebrow, headline, support, action label - that is already four.
  Everything else is an addition that dilutes: a tagline under the buttons, a trust micro-strip, a
  pricing teaser, a bullet list, a row of avatars, a status pill.
- **Plan the display size against the actual headline length.** A 3-word headline and a 12-word
  headline cannot share a font size. Write the copy, then size the type.
- One primary action. A secondary action is allowed; a third is a decision the page failed to make.

---

## Section variety

The most reliable structural tell is **repetition of one layout family**.

- A layout family appears **at most once per page**. Eight sections means at least four distinct
  families.
- **At most 2 consecutive** image-and-text splits. A third alternation reads as a template loop.
- Layout families: full-bleed statement, asymmetric split, uneven grid, centered narrow column,
  horizontal scroll or marquee, stacked list, overlapping layers, table or comparison, single large
  visual.

**Eyebrow restraint.** At most one small-caps label per three sections. Mechanical check: count the
uppercase-tracked labels, compare against `ceil(sections / 3)`, delete the excess.

**No split headers.** A heading on the left and its own supporting paragraph on the right, as a
section header, is a template pattern. Stack them.

---

## Cards

A card means **"this content is elevated above the page."** If everything is a card, nothing is
elevated and the card has become a container of last resort.

- Chopping a page into equal cards is the most common way a design stops having a hierarchy.
- **Never three equal cards side by side on a marketing page.** Content is rarely three parallel
  things of equal weight, and where it genuinely is, an uneven grid says more.
- One radius scale for the whole project: all sharp, all soft, or all pill. A mixed rule is allowed
  only if it is documented in `DESIGN.md` and tied to meaning (interactive vs static, for instance).
- **Tint shadows toward the background hue.** A neutral gray shadow on a warm surface reads as dirt.
  A shadow is occluded light, so it takes the color of what is around it.

---

## Uneven grids

An uneven grid earns its keep only when cell sizes reflect content importance.

```css
.grid {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  gap: var(--space-4);
}
.cell-hero  { grid-column: span 4; grid-row: span 2; }
.cell-wide  { grid-column: span 4; }
.cell-small { grid-column: span 2; }
```

- **Exactly as many cells as there is content.** A cell added to complete a rectangle is filler, and
  filler is visible.
- At least two or three cells need genuinely different internal treatment - a number, an image, a
  chart, a list. Same-shaped text in every cell is a table pretending to be a composition.
- Specify how it collapses on mobile, per section. "It stacks" is not a specification.

---

## Asymmetry

Above `DESIGN_VARIANCE` 4, centering everything is the safe choice that reads as no choice.

- Balance a heavy element with **empty space**, not with another heavy element.
- Useful asymmetries: large left with small right; top-heavy with sparse below; content anchored to
  one edge with negative space in the middle.
- **Below 768px, asymmetry collapses.** A phone is a single column; asymmetric compositions become
  arbitrary indentation. Override deliberately.

In PRODUCT mode, asymmetry is subordinate to scanability. A dashboard's job is a predictable place
to look.

---

## Measuring the realized dials

Read at phase 4. `DESIGN_VARIANCE` and `VISUAL_DENSITY` are declared in phase 1 and have to be
checkable against the build, or they were decoration. Count, do not estimate: an impression of your
own work always reports the number you intended.

### Realized `DESIGN_VARIANCE`

| Realized | What the build actually shows |
| :---: | --- |
| 1-2 | Every section is one centered column at the same width. |
| 3-4 | One or two sections differ. Every grid divides into near-equal fractions. |
| 5-6 | A third or more of the sections use a different layout family. At least one grid is genuinely unequal. |
| 7-8 | Most sections use a different family, at least one element breaks the container (bleed, edge-anchor, overlap), and at least one grid ratio is 2:1 or wider. |
| 9-10 | No two sections share a family. Elements overlap, rotate, or sit off the grid on purpose. |

**A near-equal split is symmetry.** `1.15fr / 0.85fr` is a 1.35:1 ratio and reads as two equal
columns with a rounding error. Nothing below **1.5:1** counts as asymmetry for this rubric. This is
the most common way a build lands at variance 3 while its lock says 8: the grid was technically
uneven and visually centered.

Count a layout "family" by what the eye sees, not by the CSS: one-column prose, two-column split,
uneven multi-cell grid, stacked list, full-bleed band, table. Two sections with the same family and
different content are one family.

### Realized `VISUAL_DENSITY`

| Realized | Section padding | What is visible at once, at 1280 |
| :---: | --- | --- |
| 1-2 | 8rem or more | One idea per screen. |
| 3-4 | 6-8rem | Two or three elements. |
| 5-6 | 4-6rem | A primary element plus supporting detail. |
| 7-8 | 2-3rem | Several data regions simultaneously. |
| 9-10 | 1.5rem or less | No space that is not carrying something. |

### Reporting

Report as `declared v/m/d, realized v/m/d`. Motion's realized value comes from the budget table in
`motion.md`: find the row whose contents match what is actually built, not what was planned.
Deviation of 2 or more on any axis is signature `A7` in `anti-slop.md` and returns the work to
phase 1.

---

## Z-index

Three named layers, in `DESIGN.md`, and nothing outside them.

```css
:root { --z-raised: 10; --z-overlay: 100; --z-grain: 1000; }
```

A `z-index: 9999` is a stacking-context bug being outrun rather than fixed.

Full-viewport texture overlays are `position: fixed; inset: 0; pointer-events: none;` and nothing
else. Without `pointer-events: none` the entire page stops accepting clicks.
