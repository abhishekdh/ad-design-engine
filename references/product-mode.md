# Product mode

**Read at:** phase 1 and phase 3, when mode is PRODUCT.

Dashboards, data tables, forms, settings, admin panels, multi-step flows, editors, any authenticated
screen. Something is being *operated*, repeatedly, by someone who already decided to use it.

The distinction that matters: a marketing page has to be **noticed**. A product screen has to be
**forgettable while it is being used**. A person is trying to reconcile an invoice, not appreciate a
composition. Craft here shows up as the absence of friction, not the presence of personality.

Most published design guidance is written for marketing pages and quietly assumes it. Applied to a
dashboard, it produces something expressive and unusable.

---

## Dial baselines

```
DESIGN_VARIANCE  4     restrained; consistency beats novelty
MOTION_INTENSITY 3     feedback only
VISUAL_DENSITY   6     information-forward
```

Adjust from there:

| Case | Var | Mot | Den |
| --- | --- | --- | --- |
| Data-dense analytics, trading, ops console | 3 | 2 | 8 |
| General SaaS application | 4 | 3 | 6 |
| Consumer app, onboarding flow | 5 | 4 | 5 |
| Internal tool, admin panel | 3 | 2 | 7 |
| Editor or canvas tool | 4 | 3 | 5 |

`DESIGN_VARIANCE` above 6 in PRODUCT mode needs an argument written into `DESIGN.md`. Above 6, the
interface starts asking to be looked at, and the person did not come to look at it.

Personality in product mode lives in **one or two places** - the empty states, the microcopy, a
single considered detail in the primary surface. Not spread across every screen.

---

## Density

`VISUAL_DENSITY` is a real dial, not a metaphor. It maps to concrete values:

| Density | Row height | Body size | Section gap | Card padding |
| --- | --- | --- | --- | --- |
| 3-4 | 3rem | 1rem | 2.5rem | 1.5rem |
| 5-6 | 2.5rem | 0.9375rem | 2rem | 1.25rem |
| 7-8 | 2rem | 0.875rem | 1.5rem | 1rem |
| 9-10 | 1.75rem | 0.8125rem | 1rem | 0.75rem |

At 8 and above, put numeric values in the monospace face and drop non-essential chrome entirely.
Density comes from removing decoration, never from shrinking targets - the 24x24 minimum holds at
every density.

---

## Tables

The table is the most-used and least-designed component in product UI.

- **Tabular figures on every numeric column.** `font-variant-numeric: tabular-nums`. Without it the
  column edges are ragged and vertical scanning, which is the entire reason a table exists, stops
  working.
- **Numbers right-aligned. Text left-aligned. Headers align with their column's data.**
- Align on the decimal by padding to a consistent decimal count within a column.
- Sticky header on any table that scrolls. `position: sticky; top: 0;` plus a `z-index` from the
  declared scale.
- Row separators, not row cards. This is one of the legitimate uses of a divider - space between
  structurally identical dense rows wastes the room the table needs.
- Zebra striping is a substitute for adequate row height. Prefer height, then a divider.
- Sortable columns show current sort state with a direction indicator, and announce it
  (`aria-sort`).
- Column widths are stable across pagination. Content-derived widths that shift on page change make
  the table feel broken.
- **Row actions:** always visible, or revealed on row hover *and* focus. Hover-only is unreachable by
  keyboard.
- Every table needs: empty, loading, error, and no-results-for-this-filter. No-results and empty are
  different states with different copy, and conflating them is the most common table bug.

---

## Forms

```
Label            above the field, always visible
Field
Help text        below the field, before the error slot
Error            below the field, replacing or joining help text
```

- **A placeholder is not a label.** It disappears on input, usually fails contrast, and breaks
  autofill. This is non-negotiable and appears in the detector at severity 4.
- One column. Multi-column forms break the reading order and are worse on every viewport.
- Group related fields with `<fieldset>` and a real `<legend>`.
- Mark **optional** fields rather than required ones when most are required, and the reverse when
  most are optional. Whichever is rarer.
- Validate on blur, not on keystroke. Validating while typing tells someone their half-entered email
  is invalid, which it obviously is.
- Errors are specific and actionable: "Password needs at least 12 characters" not "Invalid input".
- The submit button shows a loading state and is disabled during submission, and re-enables on
  failure.
- Do not disable submit until the form is valid. It hides *which* field is the problem. Let it
  submit, then focus the first error.
- Reserve vertical space for the error slot, or the whole form jumps when one appears.
- Correct `type` and `autocomplete` on every input. `type="email"`, `type="tel"`,
  `autocomplete="current-password"`. This is free and it is the difference between a two-second
  autofill and a two-minute typing session on a phone.
- `inputmode="numeric"` for numeric fields that are not numbers (codes, card numbers).

---

## Required state set

Every interactive component ships all of these. A component with only a default state is not
finished, and the missing state is always discovered in production.

```
default   hover   focus-visible   active   disabled   loading   error
```

Every data-displaying region ships all of these:

```
empty   loading   partial (some data, still fetching)   error   populated
```

Two that get skipped and both matter:

- **Empty state.** First-run is the state a new person sees first, and "No data" is a dead end. Say
  what belongs here and give the action that creates it.
- **Error state.** Says what failed, whether it is retryable, and how to retry. A generic
  "Something went wrong" is a shrug.

**Loading:** use a skeleton whose shape matches the incoming content, so nothing shifts when data
lands. A centered spinner replacing an entire region causes a layout jump on arrival and destroys
CLS. Suppress the loading state under about 300ms - a flash of skeleton reads as a glitch.

---

## Feedback timing

| Wait | Treatment |
| --- | --- |
| under 100ms | nothing; it reads as instant |
| 100-300ms | subtle state change on the control |
| 300ms-1s | skeleton or inline spinner |
| 1-10s | progress indication with what is happening |
| over 10s | background the job, notify on completion, let them leave |

Optimistic updates for actions that essentially always succeed (toggle, reorder, favourite). They
need a real rollback path, and the rollback needs to be visible - a silently reverted toggle is worse
than a slow one.

---

## State management ladder

Climb only as far as needed. Each rung is a dependency and a source of bugs.

1. **Derive it.** If a value can be computed from what you already have, do not store it. Duplicated
   state that can disagree with its source is the most common class of UI bug.
2. **Local component state.** Anything one component owns.
3. **The URL.** Filters, tabs, pagination, search, selected item. If it should survive a refresh or
   be shareable as a link, it belongs in the URL and nowhere else.
4. **Lift to the nearest common parent.** Two siblings need it.
5. **Context.** Genuinely cross-tree and rarely changing: theme, session, locale.
6. **A store.** Complex, cross-cutting, frequently-changing client state.
7. **A server-state library.** Anything from the network. Caching, revalidation, and request
   deduplication are not worth hand-writing.

**Never in component state:** pointer position, scroll offset, drag delta, animation progress. State
updates at frame rate re-render the tree at frame rate. Use a ref or a CSS custom property.

---

## Component structure

- **Container / presentation split.** One component fetches and coordinates; children take props and
  render. Presentation components are trivially testable and reusable; a component that does both is
  neither.
- **Roughly 150 lines per component, 250 hard ceiling.** Not aesthetic - a component you cannot hold
  in your head at once is a component you edit incorrectly. Past the ceiling, extract the piece with
  the clearest boundary.
- Props describe *what*, not *how*: `variant="danger"`, not `color="red"`. The token layer decides
  what danger looks like, in one place.
- Prefer composition over configuration. Five boolean props on one component means it is three
  components.
- Isolate client-interactive leaves. In a server-component architecture the client boundary goes on
  the smallest node that needs interactivity, not on the page.

---

## Navigation

- Current location is always visible and unambiguous.
- Breadcrumbs beyond two levels of depth.
- Destructive actions are separated from their neighbours, confirmed, and where possible undoable.
  Undo beats a confirmation dialog: it does not interrupt the 99% of correct actions to guard the 1%.
- Keyboard shortcuts on any repeated action, discoverable from a help affordance, never the only path.

---

## What carries over from marketing mode

Everything in `color.md`, `type.md`, `a11y.md`, and `platform.md` applies unchanged. The token
system, the OKLCH authoring, the contrast numbers, the platform features - all identical.

What changes is the budget: less variance, less motion, more density, and personality concentrated in
a few places rather than distributed everywhere.
