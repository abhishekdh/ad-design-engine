<div align="center">

# ad-design-engine

### A design skill for AI coding agents. Interfaces that read as **authored**, not generated.

A single skill that gives an AI coding agent a design process, not a pile of design opinions.
Direction before code. A gate before the build. A score before it ships.

<br>

![version](https://img.shields.io/badge/version-1.1.0-000000?style=for-the-badge)
![license](https://img.shields.io/badge/license-MIT-000000?style=for-the-badge)
![modes](https://img.shields.io/badge/modes-marketing_·_product-000000?style=for-the-badge)
![baseline](https://img.shields.io/badge/CSS-2026_baseline-000000?style=for-the-badge)
![a11y](https://img.shields.io/badge/WCAG-2.2_AA-000000?style=for-the-badge)

</div>

**ad-design-engine** is a self-contained skill for AI coding agents that turns UI work into a
process with checkpoints: classify the mode, commit to a design direction, then build against a
locked spec and score the result. It covers OKLCH-first design tokens, the current CSS platform
baseline (container queries, `@scope`, anchor positioning, scroll-driven animation, view
transitions), WCAG 2.2 AA as hard numbers rather than intentions, and a scored detector for the
seventy-odd signatures that make generated design recognizable. Two modes, expressive marketing
pages and dense product UI, because a landing page and a data table are not the same problem.

<br>

```
                      ╔══════════════════════════════════════════════╗
                      ║   the problem this exists to solve            ║
                      ╚══════════════════════════════════════════════╝

    a design rule that is never checked          →   a design rule that does not exist
    "good taste" as 700 lines of prose           →   nothing fires, nothing changes
    a beautiful landing page + an unusable table →   half the work, twice the confidence
```

Generated UI is rarely *bad*. It is **recognizable** - a set of defaults that co-occur so often they
have become a machine signature. The violet-to-pink gradient. Three equal cards. Uniform
fade-and-slide-up on every section. The em dash in every sentence. `outline: none` on focus.

This skill exists to make that signature detectable, scoreable, and expensive to ship.

---

## The pipeline

Five phases. Three gates. The gates are the entire point - a rule nobody stops at is decoration.

```
   ┌─────────────┐
   │  0  BRIEF   │   classify. echo the reading back in one line. at most one question.
   └──────┬──────┘
          │
   ┌──────▼──────┐
   │  1  DIRECT  │   set three dials. choose palette, type, layout, one bold moment.
   └──────┬──────┘   self-score. revise. present.
          │
   ══════════════════════════════════ GATE 1 ══════ ⛔ HARD STOP ═════════════════
          │                                   no code until the direction is approved
   ┌──────▼──────┐
   │  2  LOCK    │   write DESIGN.md. every value that will be reused, once.
   └──────┬──────┘
          │
   ══════════════════════════════════ GATE 2 ══════ self-check ═══════════════════
          │                                   is anything still undecided?
   ┌──────▼──────┐
   │  3  BUILD   │   tokens first. states complete. nothing invented off-plan.
   └──────┬──────┘
          │
   ┌──────▼──────┐
   │ 4 PRE-FLIGHT│   score the build. run the a11y checklist. report both.
   └──────┬──────┘
          │
   ══════════════════════════════════ GATE 3 ══════ self-check ═══════════════════
                                              does the build match what was promised?
```

**Gate 1 is a stop.** Not a checkpoint, not a summary, not a "here's the direction, now let me
build it". Direction is cheap to change and builds are not, which is the whole economic argument for
having a gate at all.

There is exactly one documented way past it without approval: **TOUCH-UP mode**, for single-component
edits, which reads the existing `DESIGN.md` as its lock, enters at phase 3, and still exits through
phase 4.

---

## Two modes, because one set of rules cannot serve both

|  | **MARKETING** | **PRODUCT** |
| --- | --- | --- |
| What it is | landing, campaign, portfolio, docs home | dashboard, table, form, settings, editor |
| The job | be remembered | be forgettable while in use |
| `DESIGN_VARIANCE` | **8** | **4** |
| `MOTION_INTENSITY` | **6** | **3** |
| `VISUAL_DENSITY` | **4** | **6** |
| Bold moment | required, exactly one | not applicable |
| Three equal cards | severity 4 hit | legitimate |
| Perpetual motion | max one element | banned |
| Personality lives in | the whole page | empty states and microcopy |

Most published design guidance is written for marketing pages and quietly assumes it. Applied to a
dashboard it produces something expressive and unusable. Applied in reverse, it produces a landing
page nobody remembers. The mode router picks first; ambiguity resolves to PRODUCT.

---

## The detector

Every phase ends in a number, not an adjective. 73 signatures, each with a severity of 1-5 and a
mode it applies in.

```
Slop score: 48 / 100  →  Generated                   Slop score: 11 / 100  →  Authored

  C1  5  violet→pink gradient, 135deg, hero            C7  2  brand chroma 0.22 on the hero
  T1  5  em dashes in 6 of 9 headings                  L9  2  padding: 13px on .card-header
  Y5  5  font-size: 5vw  (fails 200% zoom)             M9  2  ease-in-out on panel entrance
  K1  5  outline: none, no replacement                 K7  2  two icon stroke widths
  M2  5  no reduced-motion branch at intensity 6       T4  4  "seamless" in the sub-headline
  L1  4  three equal cards as the main pattern
  M1  4  fade-and-slide-up on every section          Memorable in 10s: the ledger strip in the hero.
  L3  4  4 consecutive image/text alternations       Removed: trust strip, 1 secondary CTA,
  T5  5  headline works for any competitor                    eyebrows in 3 of 7 sections.
```

Bands: **0-20** Authored · **21-40** Assisted · **41-65** Generated · **66+** Template.

A score is a claim you have to defend. Every hit names a location, because a hit without a location
cannot be fixed or verified as fixed.

### A low score is not a finished design

The detector measures the absence of known defaults, and absence is necessary rather than sufficient.
A blank page scores zero. So three of the signatures are **direction faults** that return the work to
phase 1 no matter how low the total is: a page that is about its own construction, a deliverable still
carrying placeholders, and a build whose realized dials drifted 2 or more from the ones it declared.

Phase 4 also runs four unscored pass/fail checks before the scored pass: render the build and look at
it, measure the realized dials against the declared ones, confirm a visitor can still do the job named
in phase 0, and name three elements a direct competitor's page could not contain. Each one has failed
a build that scored in single digits.

A whole section of the detector exists for the opposite of slop. Strip out every listed default and
what is left is grayscale type on white with hairline rules, monospace labels, and no material
anywhere, which scores near zero and is now its own recognizable house style. Restraint counts as a
decision when the brief asks for it and something else on the page carries the weight.

---

## Install

```bash
# personal skills directory
git clone https://github.com/abhishekdh/ad-design-engine.git \
  ~/.claude/skills/ad-design-engine

# or per-project
git clone https://github.com/abhishekdh/ad-design-engine.git \
  .claude/skills/ad-design-engine
```

The skill is plain markdown. No build step, no dependencies, no scripts.

## Use

It triggers on its own for design work. Or name it:

```
build a landing page for a freight-audit tool
redesign this settings screen
this dashboard looks templated - audit it
```

Expect a one-line reading of the brief, then a direction, then a **stop**.

---

## What is inside

`SKILL.md` (~350 lines) carries the procedure. Everything else loads only when the phase needs it -
progressive disclosure, so a simple task costs a fraction of the full corpus.

| File | Phase | What it decides |
| --- | --- | --- |
| `references/color.md` | 1, 2 | OKLCH authoring · three token layers · state derivation via `color-mix` · `light-dark()` · **banned palette clusters, by hex** |
| `references/type.md` | 1, 2 | 9 classifications, not named faces · structural pairing · ratio scales via `pow()` · **the fluid-type accessibility trap** |
| `references/layout.md` | 1, 3 | spacing as meaning · container queries · `subgrid` · hero hard limits · section variety budget |
| `references/motion.md` | 1, 3 | motion must be motivated · durations · scroll-driven animation · view transitions · `@starting-style` |
| `references/platform.md` | 3 | `@layer` · `@scope` · `:has()` · popover · **anchor positioning** · CSS-first framework theming · perf floor |
| `references/a11y.md` | 1, 4 | WCAG 2.2 AA numbers · 24×24 targets · focus at 2px/3:1 · 200% resize · the 17-line pass/fail checklist |
| `references/anti-slop.md` | 1, 4 | 73 scored signatures · four bands · three direction faults · the four questions |
| `references/product-mode.md` | 1, 3 | density→pixels table · tables · forms · required state set · state-management ladder |
| `references/marketing-mode.md` | 1, 3 | 7 hero paradigms · variety budget · imagery priority · copy discipline |
| `references/aesthetics.md` | 1 | 8 aesthetic families defined by **mechanics**, so a look can be rebuilt rather than copied |

---

## A few of the rules

Not the interesting-sounding ones. The ones that fire.

> **Fluid type must contain a `rem`.** `font-size: 5vw` and `clamp(2rem, 5vw, 4rem)` both fail the
> 200%-resize criterion, because the viewport does not change when someone zooms text. The middle term
> of `clamp()` needs a `rem` or `em` component or zoom has nothing to act on. This is the most common
> accessibility bug in otherwise careful modern CSS.

> **Zero em dashes in user-visible copy.** Severity 5. The single most reliable text-level tell in
> current generated writing.

> **Numbered markers (01 / 02 / 03) only on actual sequences.** Three parallel features are not a
> sequence, and numbering them tells the reader something false.

> **If a divider line seems necessary, the spacing is usually wrong.** A divider is a symptom of
> insufficient spacing contrast.

> **`padding: 13px` means no system is in force.** Someone nudged a value until it looked right
> instead of fixing why it looked wrong.

> **Motion claimed is motion shown.** A described interaction that was not built is the most expensive
> kind of gap, because a visitor finds it instead of a review.

> **Tint shadows toward the background hue.** A shadow is occluded light, so a neutral gray shadow on
> a warm surface reads as dirt.

---

## What it does not do

Stated plainly, because a tool that overstates its scope gets trusted in the wrong places.

- **No scripts, no linters, no CI.** The detector is a procedure an agent runs, not a binary. A real
  static analyzer for these signatures is a separate project.
- **No design-file integration.** No importing from or exporting to design tools.
- **No interchange-format emission.** `DESIGN.md` is markdown for humans and agents, not a
  machine-readable token file.
- **No framework adapters.** The platform guidance is standard CSS plus one framework-generation note.
  It does not generate components for a specific stack.
- **No slash commands.** One skill, invoked by description match or by name.
- **It cannot see.** Every visual judgement is derived from the described direction and the code. It
  narrows the space of bad output; it does not replace looking at the result.

---

## Why it is built this way

Three defects in existing design guidance drove the architecture:

1. **The generation gap.** Taste guidance ages badly. Rules written against a two-year-old CSS
   baseline recommend workarounds for problems the platform has since solved natively.
2. **The enforcement gap.** Long prose documents that declare every rule "contextual" produce nothing.
   Every rule here is a number, a binary, or a severity.
3. **The coverage split.** Guidance written for expressive marketing work either refuses product UI or
   ruins it. Hence two modes with different dial baselines and a mode-aware detector.

---

<div align="center">

**MIT** · see [`LICENSE`](LICENSE)

*A design is finished when taking anything else away would break it.*

</div>
