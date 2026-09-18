# Marketing mode

**Read at:** phase 1 and phase 3, when mode is MARKETING.

Landing pages, product sites, portfolios, campaign pages, launch announcements, agency sites,
documentation homepages. Something is being **presented** to someone who has not decided yet.

The job is a memory, not a task. Someone who leaves with nothing they can recall got nothing, even if
every element was competently rendered. This is the mode where the safe choice is the wrong one,
because the safe choice is what every generated page already looks like.

---

## Dial baselines

```
DESIGN_VARIANCE  8     take a position
MOTION_INTENSITY 6     motion is part of the argument
VISUAL_DENSITY   4     space is the luxury signal
```

Presets:

| Case | Var | Mot | Den |
| --- | --- | --- | --- |
| Portfolio, agency, creative studio | 9 | 7 | 3 |
| Product launch, campaign page | 8 | 6 | 4 |
| Developer tool landing page | 6 | 4 | 6 |
| Documentation home | 5 | 3 | 6 |
| Enterprise or regulated-industry site | 5 | 3 | 5 |
| Minimalist, restrained SaaS | 5 | 3 | 3 |

At `DESIGN_VARIANCE` 8 and above, an unconventional structural choice is **required**, not optional.
A conventional page at variance 8 has failed the dial.

---

## Brief inference

The brief is usually a sentence. Read the rest from these signals rather than asking:

| Signal | Read |
| --- | --- |
| Domain vocabulary (ledger, dose, latency, tenancy, tasting) | subject-specific structural metaphors are available |
| Named audience (developers, clinicians, founders, parents) | vocabulary register and density ceiling |
| Existing assets mentioned (logo, photos, palette) | constraints, and possibly the whole direction |
| A stated feeling (calm, urgent, premium, playful) | the vibe, taken directly |
| A named competitor or "like X but…" | the axis they want to differ on, not the thing to copy |
| Nothing at all | infer from the subject; echo the reading; ask at most one question |

Echo the reading before designing. One line, per the pipeline in `SKILL.md`. A wrong inference caught
in one line costs nothing; a wrong inference caught after a full build costs the build.

---

## Hero

The hero is where the direction either exists or does not. Hard constraints repeated from
`layout.md`, because this is where they are broken:

- Fits the initial viewport at 1440x900 and on a 375-wide phone. Headline, support, primary action.
- Headline at most 2 lines. Support at most about 20 words.
- **At most 4 text elements total.** Eyebrow, headline, support, action label is already four.
- One primary action. One optional secondary. A third means no decision was made.
- Size the display type against the actual headline. Write the copy first.

Paradigms, so the hero is a choice rather than a default:

| Paradigm | Structure | Fits |
| --- | --- | --- |
| Typographic statement | headline dominates, no imagery, extreme scale | strong copy, opinionated products |
| Split | copy one side, single visual the other | a product with something to show |
| Full-bleed visual | image or video behind, minimal overlay text | photography-led, physical products |
| Product-forward | the interface itself is the visual, cropped and oversized | tools where the UI is the argument |
| Data or artifact | a real number, chart, or object as the hero element | subjects with a concrete proof point |
| Editorial | headline plus a first paragraph, like an article opening | long-form, research, essays |
| Deconstructed | the subject's own material (code, notation, a ledger, a schematic) as composition | technical or craft subjects |

Pick against the subject. The default two-column split with a floating browser mockup is the single
most common hero in generated output.

---

## Section structure

Plan the whole page before writing any of it. A page assembled section by section drifts into
repetition, because each individual section looks fine.

Variety budget:

- A layout family appears **at most once**. Eight sections needs four or more families.
- **At most 2 consecutive** image-and-text splits.
- Vertical rhythm varies. If every section has the same height and the same padding, the page reads
  as a list even when the content is not one.
- At most one small-caps eyebrow per three sections: `ceil(sections / 3)`.
- No heading-left / paragraph-right section headers. Stack them.

Families: full-bleed statement, asymmetric split, uneven grid, centered narrow column, horizontal
scroll or marquee, stacked list, overlapping layers, comparison table, single large visual.

**One bold moment per page.** Exactly one. It can be a scroll-driven sequence, an oversized type
treatment, an unexpected layout break, a single piece of motion nobody expects. Two bold moments
compete and both lose; zero is a competent page nobody remembers. Name it in `DESIGN.md` before
building.

---

## Imagery

In priority order:

1. **Real product, real screenshots, real photography.** Nothing beats an actual artifact.
2. **The subject's own material as composition** - a code sample set as type, a document, a schematic,
   a chart, a physical object scanned. Specific to this subject and impossible to swap out.
3. **Abstract graphics built from the design system** - shapes from the palette, generated patterns,
   type as image.
4. **Illustration**, if a consistent style exists across every instance.
5. Generic stock photography. Actively harmful. It says the same thing on a thousand other pages.

Never: a floating browser window containing a generic dashboard, unless it is a real screenshot of the
real thing.

Texture, used deliberately, is one of the cheapest ways a page stops looking flat and generated: film
grain, paper fiber, halftone, subtle noise. As one deliberate layer, at low opacity, `position: fixed;
inset: 0; pointer-events: none;`. Never on top of body text.

---

## Copy

Copy is design. Ninety percent of "this looks generic" is the words.

- **Specific over impressive.** "Reconciles 40,000 transactions in under a second" beats "blazing-fast
  performance". A checkable claim reads as confidence; an unfalsifiable one reads as filler.
- **Zero em dashes.** Zero `—`, and no `–` used as a separator, in any user-visible string. Severity 5
  in the detector. Use a period, a colon, a comma, or parentheses.
- Banned constructions: "not just X, but Y", escalating triads, and the vague power words
  (seamless, robust, cutting-edge, elevate, unlock, transform, revolutionize, leverage, empower).
- The headline must be unusable by a competitor. If it works unchanged for another product in the
  category, it says nothing.
- No emoji as section markers or bullet icons.
- No invented metrics, fake logos, or placeholder testimonials presented as real. Use an obvious
  placeholder or omit the section.
- Match the register to the audience. Developer audiences read marketing register as evasion.

---

## Motion in marketing mode

`MOTION_INTENSITY` 6 is the baseline, which permits scroll-driven reveals and one continuous element.
Full budget in `motion.md`. Two rules specific to this mode:

- **Not every section animates on entry.** Uniform fade-and-slide-up on every section is the tell.
  Choose the two or three moments that carry meaning.
- **Motion claimed is motion shown.** If the plan says a headline is magnetic or a number counts up,
  the build does it. A described interaction that does not exist is discovered by a visitor, not by a
  review.

Reduced motion is mandatory at this intensity.

---

## Conversion mechanics that are not slop

- One primary action, repeated in the same visual form wherever it appears. A different-looking button
  for the same action reads as a different action.
- Social proof, if it is real, near the primary action rather than in a dedicated band.
- A page with more than about seven sections is usually two pages.
- The footer is a real part of the design. It is the last thing seen and it is where the sitemap,
  the legal surface, and the final action live.

---

## Before presenting

Run the detector in `anti-slop.md`, then answer the four questions:

1. What does someone remember 10 seconds after closing this?
2. Could this be any other product in this category?
3. What did I remove?
4. What is here that I have not shipped before?

Then remove one accessory. Cut the single element that serves the design least. A design is finished
when taking anything else away would break it.
