# Type

**Read at:** phase 1 (choosing), phase 2 (locking).

Typography carries more personality than color does. A palette can be swapped and the design
survives; change the type and it is a different product.

This file names **classifications**, not specific faces. A classification tells you what a face will
do in a layout, which is the part that transfers. Pick an actual face inside the chosen class from
whatever library the project has.

---

## Classifications and what each one does

| Class | Recognize it by | Reads as | Use for |
| --- | --- | --- | --- |
| **Neo-grotesque sans** | tight apertures, near-horizontal terminals, low contrast, narrow default spacing | neutral, systematic, corporate-safe | dense UI, tables, anything where the type must disappear |
| **Grotesque sans** | slightly quirky letterforms, more visible stroke modulation, wider spacing | engineered, contemporary | product UI that still wants character |
| **Geometric sans** | circular `o`, single-storey `a`, uniform stroke | constructed, architectural, cool | display and headlines; poor for long body text |
| **Humanist sans** | calligraphic skeleton, open apertures, varied stroke widths | warm, readable, approachable | body text at length, content-heavy pages |
| **Transitional serif** | vertical stress, moderate contrast, bracketed serifs | institutional, trustworthy, editorial | long-form reading, authority |
| **Didone serif** | extreme thick-thin contrast, hairline serifs, vertical stress | luxury, fashion, high drama | display only, and only large; illegible small |
| **Slab serif** | heavy rectangular serifs, low contrast | sturdy, mechanical, confident | headlines, wayfinding, technical |
| **Monospace** | fixed advance width | technical, precise, data | code, tabular numbers, small labels |
| **Variable grotesque** | one file, continuous weight and often width axes | flexible | any project where the weight scale needs more than 3 steps |

---

## How many faces

**One or two. If two, make them unmistakably different.**

Two faces from the same class is the worst outcome: the reader feels an inconsistency without being
able to name it, and you paid two font downloads for it. A neo-grotesque paired with a grotesque
is a bug, not a pairing.

Pairings that work, by structural contrast:

| Display | Body | Why the contrast reads |
| --- | --- | --- |
| Geometric sans | Humanist sans | constructed vs calligraphic |
| Didone serif | Neo-grotesque sans | maximum stroke contrast vs none |
| Slab serif | Humanist sans | heavy rectangular vs open and light |
| Grotesque sans | Monospace | proportional character vs fixed grid |
| Transitional serif | Monospace | traditional vs technical |

**One face is a legitimate and often better answer.** A single variable grotesque across the whole
project, with weight and size doing all the work, is harder to get wrong and looks more deliberate
than a bad pairing. Do not add a second family to prove effort.

---

## The default trap

The most-used interface sans of the last several years is now itself a tell, and so is its most
common successor. Not because either is bad - both are excellent - but because reaching for them is
what happens when no decision is made.

Two rules follow:

- **Do not pick the face you would pick for any project.** Pick the class the *subject* wants, then
  a face in it.
- **A very widely deployed face with no companion and no customization** (no tracking adjustment, no
  optical size handling, default weights only) is a signal that the type was not designed. If a
  ubiquitous face is genuinely right, make its use deliberate and say so in `DESIGN.md`.

---

## Serif discipline

A serif display face over a sans body is the single most common "we made it look designed" move.
It works. It also currently appears on an enormous number of generated pages, usually alongside a
warm cream background.

- Do not default to it. It must be argued for from the subject.
- If a serif is right, avoid the two or three most-deployed free display serifs; the whole point of
  a serif display is distinctiveness, and a recognizable one defeats it.
- **Didone at small sizes is a bug.** Hairlines disappear under about 32px on most screens, and
  entirely on low-DPI displays.

---

## Scale

Build the scale from a ratio, do not pick sizes.

```css
:root {
  --ratio: 1.2;                                  /* minor third - dense, product-safe */
  --step-0: 1rem;
  --step-1: calc(var(--step-0) * var(--ratio));
  --step-2: calc(var(--step-1) * var(--ratio));
}
```

Or compute directly with `pow()`:

```css
:root {
  --ratio: 1.25;
  --step-3: calc(1rem * pow(var(--ratio), 3));
  --step-4: calc(1rem * pow(var(--ratio), 4));
}
```

| Ratio | Name | Character | Fits |
| --- | --- | --- | --- |
| 1.125 | major second | very tight, many usable steps | dense data UI |
| 1.2 | minor third | tight, controlled | product UI, dashboards |
| 1.25 | major third | balanced | general purpose |
| 1.333 | perfect fourth | dramatic | marketing, editorial |
| 1.5 | perfect fifth | very dramatic, few usable steps | display-led pages |

A scale needs **5 to 7 steps in use**. More means the hierarchy is being expressed by size when it
should be expressed by space or weight.

---

## Fluid type, and the accessibility trap

```css
/* Correct: a base value that a viewport term ADJUSTS */
h1 { font-size: clamp(2rem, 1.5rem + 3vw, 4rem); }
```

```css
/* WRONG - never ship this */
h1 { font-size: 5vw; }
h1 { font-size: clamp(2rem, 5vw, 4rem); }   /* also wrong, see below */
```

**Why.** A font size expressed purely in viewport units does not respond to browser zoom or to a
user's default font-size setting, because the viewport does not change when the user zooms text.
A user who needs 200% text gets nothing. That is a direct failure of the 200%-resize requirement,
and it is the most common accessibility bug in otherwise careful modern CSS.

The fix is structural: **the middle term of `clamp()` must contain a `rem` or `em` component**, so
zoom always has something to act on.

```css
/* min      preferred: base + viewport adjustment      max */
clamp(1rem,  0.875rem + 0.4vw,                        1.25rem)
```

The same applies to container units. `5cqi` alone has the same defect; `0.875rem + 1cqi` does not.

Body text should usually not be fluid at all. `1rem` respects the user's own setting, which is
better than any curve you can design.

---

## Line length, height, and spacing

| Property | Value | Note |
| --- | --- | --- |
| Measure, sans body | 45-75 characters | `max-width: 65ch` is a reliable default |
| Measure, serif body | 60-80 characters | serifs tolerate longer lines |
| Line height, body | 1.5-1.65 | serif body takes the higher end |
| Line height, display | 1.0-1.15 | large type needs less |
| Line height, UI labels | 1.2-1.4 | |
| Tracking, large display | slightly negative | large type looks loose at default spacing |
| Tracking, small caps/labels | slightly positive | small type looks cramped |
| Tracking, body | leave alone | |

```css
h1 { line-height: 1.05; letter-spacing: -0.02em; }
p  { line-height: 1.6;  max-width: 65ch; }
```

**Descender clearance.** Any display line with a line-height at or below 1.1 will clip descenders
(`g j p q y`) if the container has no bottom breathing room, and italics clip worse because they
lean into the edge. Add bottom padding on tight display lines rather than raising the line-height
and losing the tightness.

---

## Line breaking

```css
h1, h2, h3 { text-wrap: balance; }   /* evens ragged lines - headings only */
p          { text-wrap: pretty; }    /* prevents orphans - body */
```

`balance` is for short blocks; browsers cap the number of lines it will operate on, so it is
pointless on paragraphs and correct on headings. `pretty` is the paragraph-level tool.

Never hand-break a headline with `<br>` to control its shape. It is correct at exactly one viewport
width and wrong at all others.

---

## Numbers

```css
.tabular { font-variant-numeric: tabular-nums; }
```

**Any column of numbers that will be compared needs tabular figures.** Proportional digits make
column edges ragged, which is the entire reason a table exists. This applies to prices, counts,
metrics, timestamps, and durations - anywhere the eye scans vertically.

At `VISUAL_DENSITY` 8 and above, put numeric values in the monospace face outright.

---

## Emphasis

- Emphasize with **italic or weight from the same family**. Never inject a different family for one
  word inside a headline.
- **Do not color or italicize a single word in a headline for emphasis.** This is one of the most
  reliable generated-design tells that exists. If a word needs that much emphasis, the sentence is
  wrong.
- All-caps labels are a default. Each one needs a reason, and a page needs very few.
- A label above a heading that repeats what the heading says is decoration wearing a functional
  costume. Cut it.

---

## Loading

- Self-host. A build-time font pipeline or a local `@font-face` both work; a third-party stylesheet
  link in production adds a DNS lookup and a render-blocking request to the critical path.
- `font-display: swap`.
- Preload only the faces used above the fold, and only the weights actually used.
- Prefer one variable font over four static weights. Usually smaller, and it unlocks intermediate
  weights for free.
- Subset to the character ranges the project needs.

```css
@font-face {
  font-family: "Display";
  src: url("/fonts/display.woff2") format("woff2-variations");
  font-weight: 300 800;
  font-display: swap;
}
```

---

## Locks

Record in `DESIGN.md`: the classes chosen, the actual faces, the roles, the ratio by name and
number, the steps in use, and the measure. A later session must be able to add a page without
re-deriving any of it.
