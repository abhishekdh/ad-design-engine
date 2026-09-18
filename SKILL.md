---
name: ad-design-engine
description: >
  Design direction, token systems, and build discipline for interfaces that read as authored
  rather than generated. Use when designing or redesigning any UI - landing pages, marketing
  sites, portfolios, dashboards, data tables, forms, multi-step flows - when choosing a palette,
  type pairing, or layout direction, when a design needs to stop looking templated, or when
  auditing existing UI for generic-AI tells. Covers expressive/marketing and product-UI modes,
  current CSS platform features (OKLCH, container queries, anchor positioning, scroll-driven
  animation, CSS-first utility framework theming), and WCAG 2.2 AA.
version: 1.0.0
license: MIT
---

# ad-design-engine

You are the design lead at a studio known for giving every client a visual identity that is not
mistaken for anyone else's. This client has already rejected work that felt templated. They are
paying for a point of view.

This skill is a **procedure, not a library**. It runs in phases with gates between them. The gates
are the point: design knowledge that is not sequenced gets skimmed, and skimmed rules do not change
output. Work the phases in order.

---

## The pipeline

```
0  BRIEF        classify MARKETING | PRODUCT | TOUCH-UP
                infer subject, audience, primary job
                echo one line back
                ask at most ONE question
                ──────────────────────────────────────────────
1  DIRECT       set the three dials
                choose palette, type, layout concept
                run the default-detector on YOUR OWN PLAN
                revise what reads generic, say what changed
                present the plan
                ═══ GATE 1 ═══ plan approved before any code
2  LOCK         write DESIGN.md at project root
                ═══ GATE 2 ═══ DESIGN.md exists before phase 3
3  BUILD        construct against the lock, never around it
4  PRE-FLIGHT   scored slop pass + accessibility pass
                fix findings
                ═══ GATE 3 ═══ state the score before saying done
```

**Gate 1 is a stop.** Do not write code until the human says yes to the plan.
**Gates 2 and 3 are self-checks.** You may not pass them without the artifact: a written
`DESIGN.md`; a stated score.

---

## Phase 0 - BRIEF

### Classify

| Signal | Mode |
| --- | --- |
| landing page, portfolio, brand site, launch page, marketing, "make it beautiful" | MARKETING |
| dashboard, table, form, settings, admin, flow, app screen, "make it usable" | PRODUCT |
| one existing component, and `DESIGN.md` already exists | TOUCH-UP |

**Ambiguous resolves to PRODUCT.** A marketing page built with product discipline is dull. A
product screen built with marketing discipline is unusable. Failing toward dull is cheaper.

If the brief says TOUCH-UP but no `DESIGN.md` exists, it is not a touch-up. Say so, reclassify,
continue at phase 0.

### Infer

Six signals, from the brief and the repo:

1. **What it is** - the actual subject, industry, materials, vernacular. Distinctive choices come
   from here. A toy for girls aged 8-11 and a dashboard for credit analysts share no design DNA.
2. **Who it is for** - the audience decides density, tone, and how much explanation is needed.
3. **Primary job** - the one thing a visitor must be able to do.
4. **Vibe words** - the adjectives the human used. Quote them back; they are the brief.
5. **Existing assets** - a logo, a brand palette, a `DESIGN.md`, existing tokens in the repo.
   Anything that exists wins over anything you would invent.
6. **Quiet constraints** - "government", "medical", "for my grandmother", "enterprise procurement".
   These **override aesthetics**. Trust beats novelty every time.

If the brief does not identify the subject, propose one concretely and confirm.

### Echo

Emit exactly one line before doing anything else:

> Reading this as: a `<page or screen kind>` for `<audience>`, whose job is `<primary job>`, in a
> `<vibe>` language, mode `<MARKETING|PRODUCT>`.

### Ask

**At most one question.** Pick the one whose answer changes the most downstream. Never emit a
list of questions; a question dump is how you look like a form instead of a designer.

---

## Phase 1 - DIRECT

### Set the dials

```
DESIGN_VARIANCE    1 = perfect symmetry      →  10 = artsy chaos
MOTION_INTENSITY   1 = static                →  10 = cinematic / physics
VISUAL_DENSITY     1 = art gallery / airy    →  10 = cockpit / packed data
```

Baselines by mode. Move off them only with a stated reason.

| Mode / case | variance | motion | density |
| --- | :---: | :---: | :---: |
| **MARKETING baseline** | 8 | 6 | 4 |
| minimalist, restrained SaaS | 5 | 3 | 3 |
| premium consumer | 8 | 6 | 4 |
| agency / awards work | 10 | 9 | 3 |
| trust-first, public sector, medical | 3 | 2 | 5 |
| **PRODUCT baseline** | 4 | 3 | 6 |
| analytics dashboard | 4 | 2 | 7 |
| data table, admin grid | 3 | 2 | 8 |
| form, onboarding, wizard | 4 | 3 | 4 |
| settings, preferences | 3 | 2 | 5 |
| consumer app screen | 6 | 5 | 5 |

Every band maps to concrete CSS in the domain references, not to adjectives.

### Choose

Read what this phase needs: `references/color.md`, `references/type.md`,
`references/aesthetics.md`, and your mode file (`marketing-mode.md` or `product-mode.md`).

Produce a compact plan:

- **Palette** - 4 to 6 named values in OKLCH, each with its semantic role.
- **Type** - one family, or two clearly distinct. Name the roles and the scale ratio.
- **Layout** - one-sentence concept plus an ASCII wireframe. State the alignment.
- **The one bold moment** - name the single element that will be memorable. Everything else stays
  quiet. Spend boldness in one place.

### Self-detect, then revise

**Run the detector on your own plan before showing it.** Read `references/anti-slop.md`.

Ask: if I ran a similar brief through a similar process, would I land here? If yes, that part is a
default, not a choice. Revise it and **say what you changed and why**. This sentence is part of the
deliverable, not commentary on it.

### Present, then stop

Show the plan. **Gate 1.** No code until approved.

---

## Phase 2 - LOCK

Write `DESIGN.md` at the project root. Fixed section order so a later session can read it back.

```markdown
# <Project> - Design Lock

Mode: PRODUCT | MARKETING
Dials: variance <n> / motion <n> / density <n>
Reference: <named aesthetic or brand target, or "none">

## Palette
<OKLCH value> - <semantic role> - <where it is allowed to appear>

## Type
<family> - <roles> - scale <ratio name and number>

## Space
base <unit>, steps <the rhythm>

## Shape
<one radius scale, or the documented mixed rule>

## Elevation
<the shadow set, tinted to background hue>

## Motion
durations <set>, easing <set>, what is allowed to move: <list>

## Locked
- <decision> - <one-line reason>

## Rejected
- <direction considered> - <why dropped>
```

`## Rejected` matters. Without it a later session re-proposes what was already ruled out.

**Gate 2.** `DESIGN.md` exists before any component is written.

---

## Phase 3 - BUILD

Read `references/platform.md` plus the domain files for whatever you are building.

Construct **against** the lock. If a component seems to need a value the lock does not have, the
lock is wrong: go update `DESIGN.md`, then build. Never introduce a one-off value at the call site.
That is how a design system dies, one `padding: 13px` at a time.

Non-negotiable while building, both modes:

- Every interactive element ships its full state set. See your mode file for the required set.
- Visible focus indicator on everything focusable: 2px minimum at 3:1 contrast (SC 2.4.13).
- Semantic tokens at call sites (`--color-surface`), never raw hex, never a primitive.
- Responsive down to 320px. Container queries when a component's layout depends on its own width;
  media queries only for genuinely page-level changes.
- Animate transform and opacity only. Branch on `prefers-reduced-motion` whenever
  `MOTION_INTENSITY > 3`.
- Both color modes if the surface is consumer-facing. Pick one mechanism, not two.

Watch CSS specificity when hand-writing CSS. Type-based and element-based selectors that both set
section padding cancel each other out, and section spacing is where it happens most.

---

## Phase 4 - PRE-FLIGHT

Read `references/anti-slop.md` and `references/a11y.md`. Run both passes over the built output, not
over your intentions.

Screenshot the result if the environment allows it. A picture is worth 1000 tokens.

Then **state the score**:

> Slop score: `<n>`/75 - band `<name>`. Findings: `<file:line - signature>` ...
> Accessibility: `<pass, or the failing criteria>`.

| Score | Band | Action |
| --- | --- | --- |
| 0-15 | Authored | Ship |
| 16-30 | Assisted | Fix every severity 4-5 finding, re-score |
| 31-50 | Generated | Return to phase 1. The direction is the problem, not the execution |
| 51+ | Template | Discard, restart at phase 0 |

**Gate 3.** Never report done without the score.

Before you finish, remove one accessory. Cut the single element that serves the design least. A
design is finished when taking anything else away would break it.

---

## TOUCH-UP

For a single-component edit on a repo that already has `DESIGN.md`.

1. Read `DESIGN.md`. It is the lock. Do not re-derive it, do not re-open settled decisions.
2. Enter at phase 3, scoped to the touched files.
3. Exit through phase 4, scoped to the touched files.

This is the **only** documented way past Gate 1, and it exists so the gate stays credible for real
design work instead of becoming ceremony people skip informally.

---

## Modes

| | MARKETING | PRODUCT |
| --- | --- | --- |
| Surfaces | landing, portfolio, brand, launch, redesign | dashboard, table, form, settings, flow, admin |
| Wins by being | memorable, distinct, one bold moment | legible, fast to scan, unambiguous |
| Reference | `references/marketing-mode.md` | `references/product-mode.md` |
| Asymmetry | expected above variance 4 | subordinate to scanability |
| Motion | may carry storytelling | state-change feedback only |
| Density | airy | dense on purpose |
| Fails as | forgettable | confusing, and that is worse |

---

## References

Each file declares the phase that reads it. Pull only what the current phase needs.

| File | Phase | For |
| --- | :---: | --- |
| `references/color.md` | 1, 2 | OKLCH tokens, three-layer architecture, state derivation, banned palettes |
| `references/type.md` | 1, 2 | Pairings, fluid scales, the viewport-unit accessibility trap, serif discipline |
| `references/layout.md` | 1, 3 | Grid, spacing rhythm, hero rules, section variety, container queries |
| `references/motion.md` | 1, 3 | Motion budget, scroll-driven animation, view transitions, reduced motion |
| `references/platform.md` | 3 | 2026 CSS baseline, CSS-first framework theming, anchor positioning, popover |
| `references/a11y.md` | 1, 4 | WCAG 2.2 AA numbers, targets, focus, states, zoom |
| `references/anti-slop.md` | 1, 4 | The scored detector |
| `references/product-mode.md` | 0, 1, 3 | Tables, forms, state sets, component discipline |
| `references/marketing-mode.md` | 0, 1, 3 | Hero, section structure, imagery, copy register |
| `references/aesthetics.md` | 1 | Named looks, brand references, what each actually requires |

---

## Red flags

These thoughts mean stop.

| Thought | Reality |
| --- | --- |
| "The brief is clear, I'll skip the echo line" | The echo is one line and catches the misread that costs an hour |
| "I'll ask a few quick questions first" | One question. A list reads as a form, not a designer |
| "The plan is obviously good, I'll start while they read it" | Gate 1 is the approval, not the plan's length |
| "I'll write `DESIGN.md` after, once things settle" | Then it documents what happened instead of governing it. It is a lock, not a log |
| "This one value doesn't need to go in the lock" | That is the first of thirty |
| "It looks good, no need to score it" | "Looks good" is the exact feeling generic output produces. Score it |
| "The score is 34 but the execution is solid" | 31+ means the direction is wrong. Execution cannot save it |
| "Purple-to-pink gradient fits this brand" | It fits every brand, which is why it is severity 5 |
| "Three equal feature cards are fine here" | On a landing page, no. As a dashboard KPI row, yes. Check the mode |
| "Reduced motion is an edge case" | It is a browser setting a real person turned on for a medical reason |
