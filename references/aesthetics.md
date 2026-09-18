# Aesthetic families

**Read at:** phase 1, when a direction needs a starting point.

A vibe word is not a direction. "Premium", "clean", "bold" each describe a dozen incompatible
designs, and asking for one of them produces the generic average of all of them.

This file breaks aesthetics into **families defined by their mechanics** - the actual palette, type
class, spacing, and motion decisions that make each one read the way it does. A family is a starting
position, not a destination: pick one, then push it toward the subject.

Deliberately no reference names here. A named target invites imitation, which is both a legal
question and a design failure - the point is to know *why* a look works so it can be rebuilt from the
subject rather than copied from an example.

---

## How to use a family

1. Read the subject and pick the family whose *intent* matches, not the one that looks nicest.
2. Take its mechanics as the starting palette, type class, and spacing.
3. **Change at least two of its mechanics** in a direction the subject dictates. A family applied
   unmodified is a template.
4. Record what you changed and why in `DESIGN.md`.

---

## Swiss / systematic

Grid is visible. Type does everything. Ornament is absent.

```
Palette     near-black, white, exactly one accent. Chroma under 0.12.
Type        neo-grotesque or grotesque sans, one face, 3-4 weights
Scale       tight ratio, 1.125 or 1.2
Space       strict scale, generous, aligned to a visible column grid
Layout      asymmetric within a rigid grid; content aligned to grid lines, not centered
Motion      minimal, 1-3
Density     5-7
```

Reads as: rigorous, institutional, confident. Works for: editorial, data, technical products,
anything where authority comes from clarity.

Fails when: the grid is decorative rather than structural. Alignment must actually be enforced, or
this becomes "plain".

---

## Editorial / print

Borrowed from magazines. Text is the primary visual.

```
Palette     paper white or warm off-white, ink near-black, one muted accent
Type        two faces with real contrast: serif display over sans body, or the reverse
Scale       dramatic ratio, 1.333 or 1.5
Space       generous vertical rhythm, narrow measure (60-70ch), asymmetric margins
Layout      pull quotes, drop caps, multi-column at width, images breaking the column
Motion      2-4; scroll-driven progress at most
Density     4-6
```

Reads as: considered, literate, slow. Works for: long-form, research, essays, journalism, agencies
selling thinking.

Fails when: the content is not actually long-form. Editorial treatment on three feature bullets is
costume.

---

## Brutalist / raw

Structure exposed. No attempt to be pleasant.

```
Palette     system default colors, high contrast, one aggressive accent, or none
Type        system stack or monospace, few weights, default metrics
Scale       extreme jumps, few steps
Space       tight or wildly inconsistent, on purpose
Layout      visible borders, unstyled defaults, no radius, no shadow
Motion      0-2; often none
Density     7-9
```

Reads as: honest, technical, anti-commercial. Works for: developer tools, art projects, anything
whose audience distrusts polish.

Fails when: accessibility is treated as part of the aesthetic. Raw does not mean 2:1 contrast or
invisible focus rings. The numbers in `a11y.md` are not negotiable in any family.

---

## Soft / humane

Warmth without sweetness.

```
Palette     low-chroma warm neutrals, one soft accent, chroma 0.06-0.12, no pure white or black
Type        humanist sans, or geometric display over humanist body
Scale       moderate, 1.2 or 1.25
Space       generous, large radii consistently applied
Layout      centered or gently asymmetric, soft cards, tinted shadows
Motion      3-5, all ease-out, no bounce
Density     4-5
```

Reads as: approachable, calm, trustworthy. Works for: health, finance-for-humans, education,
consumer products.

Fails when: it slides into the pastel-and-rounded default. This family is one step from the most
generated look there is - the differentiator is a *specific* warm hue and restraint about radius.

---

## Dark technical

Dark surface as the working environment, not as a theme.

```
Palette     dark surface floor oklch(0.14-0.20) with brand-hue chroma 0.005-0.02,
            one or two functional accents, semantic status colors
Type        grotesque sans for UI, monospace for values
Scale       tight, 1.125 or 1.2
Space       compact, strict scale
Layout      dense, panelled, borders over shadows (shadows barely read on dark)
Motion      2-3, feedback only
Density     7-9
```

Reads as: professional, focused, built for long sessions. Works for: developer tools, monitoring,
trading, editors.

Fails when: it is near-black plus one acid green and nothing in between. That specific pairing is in
the detector at severity 3. Dark technical needs a full lightness range between floor and text, not
two extremes.

---

## Luxury / restrained

Value signalled by what is absent.

```
Palette     one deep hue at several lightnesses, or cold neutrals; a metallic-adjacent
            low-chroma accent; never a gradient
Type        didone or transitional serif display, over neutral sans body;
            positive tracking on small caps
Scale       dramatic, 1.333+
Space       very generous. Emptiness is the product.
Layout      centered or strictly symmetrical, large images, minimal text
Motion      3-5, slow, long ease-out, nothing playful
Density     2-4
```

Reads as: expensive, discreet, timeless. Works for: premium goods, hospitality, high-end services.

Fails when: emptiness has nothing to frame. Luxury layouts need one exceptional image or one
exceptional line of type at the center; without it the space reads as unfinished.

---

## Maximalist / expressive

Density of ideas rather than of data.

```
Palette     3-5 hues with a documented job each, high chroma, deliberate clashes
Type        2-3 faces, extreme size contrast, type as image
Scale       very dramatic, 1.5, few steps
Space       varied, intentionally uneven
Layout      overlapping layers, rotation, full-bleed color blocks, marquees
Motion      6-9; motion is part of the composition
Density     5-7
```

Reads as: energetic, young, confident. Works for: culture, music, events, creative portfolios.

Fails when: nothing is subordinated. Maximalism still has a hierarchy - it just has three levels
instead of five. Everything loud is the same as everything quiet.

---

## Utilitarian / neutral

Deliberately unremarkable, and correct about it.

```
Palette     true neutrals with a trace of brand chroma (0.002-0.008), one functional accent
Type        one grotesque sans, 3 weights
Scale       tight, 1.125
Space       compact, uniform
Layout      predictable, consistent, repeatable
Motion      1-2
Density     6-8
```

Reads as: reliable, invisible, fast. Works for: internal tools, admin, infrastructure, anything used
all day by people who did not choose it.

Fails when: it is chosen to avoid a decision. This family is legitimate as an argued position and a
failure as a default. Say in `DESIGN.md` that it was chosen.

---

## Cross-family rules

These hold regardless of which family is picked:

- **One family per project.** Mixing is not a hybrid; it reads as inconsistency.
- Every family gets the contrast numbers, the focus ring, the 24x24 targets, and the reduced-motion
  branch. No aesthetic exempts accessibility.
- A family names the *starting* dials. The dials chosen for the project override them, and the
  override goes in `DESIGN.md`.
- If none of these fits, build a direction from the subject's own material - its vocabulary, its
  physical artifacts, its notation. A subject-derived direction beats every family in this file,
  because it cannot be applied to anything else.

---

## The test

Describe the direction in one sentence that could not describe a different project. If the sentence
would fit a competitor unchanged, the direction is a family with the content swapped in, and the
detector will find it.
