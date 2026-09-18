# Anti-slop detector

**Read at:** phase 1 (self-detect before presenting), phase 4 (score the build).

A generated design is rarely bad. It is *recognizable* - a set of defaults that co-occur so often
they now read as a machine signature. This file turns that recognition into a number.

---

## How to score

1. Walk every signature below against the artifact.
2. Add the **severity** of each one present.
3. Only count signatures whose **Modes** column includes the current mode.
4. Report the total, the band, and the individual hits with their severities.

Max realistic total: 75. Bands:

| Score | Band | Meaning |
| --- | --- | --- |
| **0-15** | Authored | Reads as a deliberate design. Ship it. |
| **16-30** | Assisted | Defaults are visible. Fix the highest severities. |
| **31-50** | Generated | Recognizable as machine output. Revise the direction, not the details. |
| **51+** | Template | This is a template with the content swapped. Start the direction over. |

**A score is a claim you have to be able to defend.** Reporting 8 while three severity-5 signatures
are present is worse than not scoring, because it launders the problem. When unsure whether a
signature is present, count it.

The mode column matters. A detector that fires on legitimate product patterns - three KPI cards, a
dense table, a centered form - gets mentally switched off, and a switched-off detector is worse than
none.

---

## Text (both modes unless noted)

| # | Signature | Sev | Modes |
| --- | --- | --- | --- |
| T1 | Any em dash (`—`) or en dash used as a sentence separator in user-visible copy | **5** | both |
| T2 | "Not just X, but Y" / "It's not about X, it's about Y" construction | 4 | both |
| T3 | Escalating triads: "faster, smarter, better" | 3 | both |
| T4 | Vague power words carrying the value proposition: seamless, robust, cutting-edge, elevate, unlock, transform, revolutionize, leverage, empower, effortless | 4 | both |
| T5 | Headline says nothing checkable - could belong to any product in any industry | 5 | MARKETING |
| T6 | Emoji as section markers or bullet icons | 3 | both |
| T7 | A single word in a headline colored or italicized for emphasis | 4 | both |
| T8 | An eyebrow label that restates the heading below it | 2 | both |
| T9 | Rhetorical question as a section heading | 2 | MARKETING |
| T10 | Invented metrics, fake logos, placeholder testimonials presented as real | 5 | both |

**On T1.** The em dash is the single most reliable text-level tell in current generated writing.
Target: **zero** `—` and zero `–`-as-separator in any user-visible string. Use a period, a colon, a
comma, or parentheses. This applies to copy, labels, tooltips, empty states, and error messages. It
does not apply to code comments or to this documentation.

---

## Color

| # | Signature | Sev | Modes |
| --- | --- | --- | --- |
| C1 | The synthetic gradient: violet-to-pink or blue-to-violet, usually at 135deg | **5** | both |
| C2 | The default blue-and-violet accent pair | 4 | both |
| C3 | The warm-cream-plus-terracotta cluster (see `color.md` for the exact hex set) | **5** | both |
| C4 | Near-black background plus exactly one acid accent, with nothing between them | 3 | both |
| C5 | A tinted near-black used as if it were neutral black | 2 | both |
| C6 | More than one accent hue with no documented job for each | 3 | both |
| C7 | Chroma above 0.20 on a large surface | 2 | both |
| C8 | Glassmorphism: translucent panel plus backdrop blur, as a default rather than a decision | 3 | both |
| C9 | Gradient text on a heading | 4 | both |
| C10 | State communicated by color alone | 3 | both |
| C11 | Dead-neutral grays (chroma exactly 0) alongside a chromatic brand | 2 | both |

---

## Type

| # | Signature | Sev | Modes |
| --- | --- | --- | --- |
| Y1 | The current default interface sans, unmodified, with no companion and no tracking work | 3 | both |
| Y2 | Two faces from the same classification | 4 | both |
| Y3 | Three or more type families | 4 | both |
| Y4 | Free display serif over neutral sans, chosen by default rather than argued for | 3 | MARKETING |
| Y5 | Fluid type with no `rem` term in the `clamp()` middle | **5** | both |
| Y6 | More than 7 distinct sizes in use | 2 | both |
| Y7 | Hand-broken headline using `<br>` | 3 | both |
| Y8 | Numeric column without tabular figures | 3 | PRODUCT |
| Y9 | Display type at line-height 1.5 or looser | 2 | both |

---

## Layout

| # | Signature | Sev | Modes |
| --- | --- | --- | --- |
| L1 | Three equal cards side by side as the primary content pattern | 4 | MARKETING |
| L2 | Every section centered, single column, same width | 4 | MARKETING |
| L3 | Alternating image-left / image-right beyond two consecutive sections | 4 | MARKETING |
| L4 | Numbered markers (01/02/03) on content that is not a sequence | 3 | both |
| L5 | Icon-in-a-rounded-square above every feature heading | 3 | both |
| L6 | Everything in a bordered card, including the primary element | 3 | both |
| L7 | Hero taller than the viewport, or more than 4 text elements in it | 4 | both |
| L8 | Uniform vertical rhythm - every section the same height and padding | 3 | MARKETING |
| L9 | A spacing value off the scale (`padding: 13px`) | 2 | both |
| L10 | Section header split into heading-left / paragraph-right | 2 | MARKETING |
| L11 | Grid cells added to complete a rectangle rather than to hold content | 3 | both |
| L12 | More eyebrow labels than `ceil(sections / 3)` | 2 | MARKETING |
| L13 | `100vh` instead of `100dvh` on a full-height element | 2 | both |

---

## Motion

| # | Signature | Sev | Modes |
| --- | --- | --- | --- |
| M1 | Uniform fade-and-slide-up on every section, roughly 30px over 300-600ms | 4 | both |
| M2 | No reduced-motion branch above `MOTION_INTENSITY` 3 | **5** | both |
| M3 | Animating a layout property (`width`, `height`, `top`, `margin`) | 3 | both |
| M4 | More than one perpetually looping element | 3 | MARKETING |
| M5 | Any perpetual motion at all | 3 | PRODUCT |
| M6 | Scroll position driven from component state | 4 | both |
| M7 | Bounce or spring easing on a product control | 3 | PRODUCT |
| M8 | Motion described in the plan but not present in the build | **5** | both |
| M9 | Symmetric easing on an entrance | 2 | both |

---

## Craft

| # | Signature | Sev | Modes |
| --- | --- | --- | --- |
| K1 | `outline: none` on focus with no replacement indicator | **5** | both |
| K2 | Interactive target under 24x24 CSS px | 4 | both |
| K3 | Placeholder used as the field label | 4 | PRODUCT |
| K4 | Missing states: no hover, or no focus, or no disabled, or no loading, or no error, or no empty | 4 | PRODUCT |
| K5 | Body text below 4.5:1, or muted text produced with `opacity` | **5** | both |
| K6 | Import of a package that is not in the manifest | **5** | both |
| K7 | Mixed icon sets, or mixed stroke widths | 2 | both |
| K8 | `z-index` outside the three declared layers | 2 | both |
| K9 | Images without `width` and `height` | 3 | both |
| K10 | Untested at 320px | 3 | both |
| K11 | Only the happy path built - no empty, loading, or error state | 4 | PRODUCT |
| K12 | Dark mode implemented by two mechanisms at once | 2 | both |

---

## The four questions

Scoring finds defaults. These find the absence of a design. Answer all four before presenting
anything.

1. **What is the one thing someone remembers 10 seconds after closing this?** No answer means there
   is no design, only an arrangement. Score is irrelevant at that point.
2. **Could this be any other product in this category?** If the palette, type, and structure would
   work unchanged for a competitor, the direction is generic regardless of score.
3. **What did I remove?** A design nobody subtracted from is a design nobody edited.
4. **What is here that I have not shipped before?** If every choice is a repeat of the last project,
   the output is a personal template rather than a machine one. Same defect.

---

## Reporting format

```
Slop score: 14 / 75  (Authored)

Hits:
  T4  4  "seamless" carries the sub-headline
  C7  2  brand chroma 0.22 on the full-bleed hero
  L9  2  padding: 13px on .card-header
  Y6  2  9 distinct sizes; 3 are within 1px of another
  M9  2  ease-in-out on the panel entrance
  K7  2  two stroke widths in the icon row

Memorable in 10s: the horizontal ledger strip in the hero.
Removed: the trust micro-strip, one of two secondary actions, the section eyebrows in 3 of 7.
```

Every hit names the location. A hit without a location cannot be fixed, and cannot be verified as
fixed.
