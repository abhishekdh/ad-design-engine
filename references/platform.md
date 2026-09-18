# Platform

**Read at:** phase 3.

The CSS baseline below is broadly available in current browsers. Everything here replaces a
JavaScript dependency or a workaround that used to be required. Reaching for the old workaround is
both more code and worse behavior.

---

## Cascade layers

```css
@layer reset, tokens, base, components, utilities;

@layer components { .btn { padding: var(--space-3) var(--space-5); } }
@layer utilities  { .p-0 { padding: 0; } }
```

Layer order beats specificity entirely. A single-class rule in a later layer wins over a
five-selector rule in an earlier one. This is what removes the specificity arms race that produces
`!important` and `.page .section .card .title` selectors.

Unlayered styles win over all layered styles, so third-party CSS can be quarantined:

```css
@layer vendor { @import url("vendor.css"); }
```

---

## Scoping

```css
@scope (.card) to (.card-body) {
  a { color: var(--color-brand); }   /* applies inside .card, stops at .card-body */
}
```

The `to` clause creates a donut hole, which is the thing no selector could previously express:
style a subtree but stop at nested boundaries. Useful for content areas that contain other
components.

---

## Nesting

```css
.card {
  padding: var(--space-6);

  & > h3 { margin-block-end: var(--space-2); }

  &:hover { border-color: var(--color-brand); }

  @container (min-width: 30rem) { grid-template-columns: 8rem 1fr; }
}
```

Nest at most two levels. Deeper nesting rebuilds the specificity problem that layers just solved,
and makes the generated selector unreadable in devtools.

---

## Relational selection

```css
/* label style depends on the input's state - previously impossible without JS */
.field:has(input:invalid) label { color: var(--color-danger); }
.field:has(input:focus-visible) { outline: 2px solid var(--color-brand); }

/* card layout depends on whether it contains an image */
.card:has(img) { grid-template-columns: 8rem 1fr; }

/* the "not empty" case */
.list:not(:has(li)) { display: none; }
```

`:has()` takes its specificity from its most specific argument and does not increase the subject's
specificity itself.

---

## Custom property types

```css
@property --gradient-angle {
  syntax: "<angle>";
  inherits: false;
  initial-value: 0deg;
}

.card { transition: --gradient-angle 400ms; }
.card:hover { --gradient-angle: 120deg; }
```

Untyped custom properties are strings, and strings cannot be interpolated - which is why animating a
gradient used to require a JS tick. `@property` gives the browser a type, so it can animate the
value. It also provides a real fallback via `initial-value`.

---

## Popover

```html
<button popovertarget="menu">Open</button>
<div id="menu" popover>…</div>
```

Free, with no JavaScript: top-layer rendering (no `z-index` problem), light-dismiss on outside click
and Escape, focus management, and `::backdrop`.

```css
[popover]::backdrop { background: oklch(0 0 0 / 0.4); backdrop-filter: blur(2px); }
```

`popover="manual"` opts out of light-dismiss for cases that must be dismissed explicitly.

---

## Anchor positioning

This replaces a positioning library outright.

```css
.trigger { anchor-name: --menu-trigger; }

.menu {
  position: fixed;                       /* required - absolute or fixed only */
  position-anchor: --menu-trigger;
  position-area: block-end span-inline-start;
  position-try-fallbacks: block-start span-inline-start, inline-end, inline-start;
  margin-block-start: var(--space-2);
}
```

- `position-area` places the element in a 3x3 grid around the anchor, which covers most cases with
  one declaration.
- `anchor()` gives precise control when the grid is not enough:
  `top: anchor(bottom); left: anchor(left);`
- `position-try-fallbacks` is the collision handling: the browser tries each option in order until
  one fits in the viewport. This is the entire reason a positioning library was needed.
- `anchor-size()` sizes the positioned element from the anchor:
  `min-width: anchor-size(width);`

**Pitfall:** the positioned element must be `absolute` or `fixed`, or none of it applies.

---

## Container queries

Covered in `layout.md`. Summary: `container-type: inline-size` on the wrapper,
`@container (min-width: …)` for the query, `cqi` for the unit, and never a bare `cqi` font-size.

---

## Scroll-driven animation and view transitions

Covered in `motion.md`.

---

## Text and sizing utilities

```css
h1, h2, h3   { text-wrap: balance; }
p            { text-wrap: pretty; }
textarea     { field-sizing: content; }
:root        { interpolate-size: allow-keywords; }
```

---

## Color functions

Covered in `color.md`: `oklch()`, `color-mix(in oklch, …)`, `oklch(from …)`, `light-dark()`, and the
required `color-scheme`.

---

## Math

```css
--step-4: calc(1rem * pow(1.25, 4));
--clamped: clamp(1rem, 0.875rem + 0.5vw, 1.5rem);
--stagger: calc((sibling-index() - 1) * 40ms);
--cols: round(down, 100cqi / 16rem, 1);
```

`pow()`, `sqrt()`, `round()`, `mod()`, `rem()`, `abs()`, and `sign()` are available.
`sibling-index()` and `sibling-count()` remove the last common reason to write inline styles in a
loop.

---

## Utility-first frameworks, v4 generation

The v4 generation of utility-first CSS frameworks moved theme configuration **out of a JavaScript
config file and into CSS**. Guidance written for the v3 generation is wrong about this, and it is the
most common source of broken setups.

In the single CSS entry file that imports the framework:

```css
@import "<the framework package>";

@theme {
  --font-display: "Display", sans-serif;
  --color-brand-500: oklch(0.55 0.18 264);
  --spacing-18: 4.5rem;
  --breakpoint-3xl: 120rem;
  --ease-fluid: cubic-bezier(0.3, 0, 0, 1);
}
```

Key behaviors:

- Each `@theme` entry generates the corresponding utilities **and** is emitted as a real CSS custom
  property, so the same token is available to hand-written CSS and to arbitrary values.
- The namespace prefix determines what is generated: `--color-*` produces color utilities,
  `--font-*` font-family, `--spacing-*` spacing, `--breakpoint-*` responsive variants,
  `--ease-*` easing.
- Clear a namespace by setting it to `initial`. This is how the default palette is removed so only
  project tokens remain:

```css
@theme {
  --color-*: initial;
  --color-surface: oklch(0.99 0.002 264);
  --color-text:    oklch(0.21 0.01 264);
}
```

- Class-based dark mode is declared, not configured:

```css
@custom-variant dark (&:where(.dark, .dark *));
```

- Use the framework's own build plugin (PostCSS or bundler). The v3-era plugin name is not the
  v4 entry point.

**Consequence for this skill:** the `DESIGN.md` palette maps directly into `@theme`. Tokens are
declared once and consumed as utilities, as custom properties, or both. There is no second source of
truth, which is the whole point.

---

## Framework-side rules

- Continuous values - pointer position, scroll offset, drag delta, physics - never live in component
  state. State updates at frame rate re-render the tree at frame rate. Use a ref, a CSS custom
  property, or the animation library's own value primitive.
- Isolate interactive leaves. In a server-component architecture, the client boundary belongs on the
  smallest node that needs it, not on the page.
- **Verify a dependency exists before importing it.** Read the manifest. An import of a package that
  is not installed is a build failure, and it is the single most common way generated UI code fails
  on first run.
- One icon set for the whole project, one stroke width, set globally. Never hand-author an icon path
  when a set is present.

---

## Performance floor

| Metric | Budget |
| --- | --- |
| Largest contentful paint | under 2.5s |
| Interaction to next paint | under 200ms |
| Cumulative layout shift | under 0.1 |

The three things that actually cause failures here: images without `width` and `height` (shift),
fonts without `font-display: swap` (paint), and animating a layout property (interaction).
