# Motion

**Read at:** phase 1 (budget), phase 3 (building).

---

## Motion must be motivated

Every animation states a reason before it is written. Valid reasons, and there are only four:

1. **Feedback** - something acknowledges an action the person took.
2. **State transition** - showing what changed, so the change is not a jump cut.
3. **Hierarchy** - directing attention to the one thing that matters most, once.
4. **Storytelling** - MARKETING mode only, and only where the narrative is the product.

Invalid reasons: it looked flat, the page felt static, everything else animates, it was easy.

**The most recognizable motion tell is uniform fade-and-slide-up on every section as it enters the
viewport, about 30px over 300-600ms, triggered once.** It is not wrong; it is what happens when
motion is applied instead of designed. One orchestrated moment lands harder than fifteen identical
reveals.

---

## What may be animated

```
transform   ✓   compositor-only, no layout, no paint
opacity     ✓   compositor-only
filter      ~    paints; acceptable on small elements, expensive full-bleed
```

Everything else triggers layout or paint on every frame. `width`, `height`, `top`, `left`, `margin`,
`padding`, `box-shadow` spread - all of these are the reason an animation stutters.

To animate size, use `transform: scale()`, or animate `grid-template-rows` / `height` **only** with
the keywords below, which are cheap because the browser resolves them once.

```css
/* animating to and from intrinsic size, which used to be impossible */
:root { interpolate-size: allow-keywords; }
.panel { height: 0; transition: height 250ms ease-out; }
.panel[open] { height: auto; }
```

`field-sizing: content` handles the textarea-grows-with-content case with no animation code at all:

```css
textarea { field-sizing: content; }
```

---

## Duration and easing

| Interaction | Duration |
| --- | --- |
| Hover, focus, small state change | 120-180ms |
| Toggle, checkbox, small reveal | 180-250ms |
| Panel, drawer, dropdown | 250-350ms |
| Page or view transition | 350-500ms |
| Deliberate storytelling beat | 600-1200ms |

Anything above 500ms in a product interface feels broken, because the person is waiting on the
animation rather than reading the result.

```css
:root {
  --ease-out:   cubic-bezier(0.16, 1, 0.30, 1);   /* decelerate - entering, revealing */
  --ease-in:    cubic-bezier(0.55, 0, 1, 0.45);   /* accelerate - leaving */
  --ease-inout: cubic-bezier(0.65, 0, 0.35, 1);   /* both - moving between positions */
}
```

Entering uses ease-out: fast start, settled finish. Leaving uses ease-in. Symmetric easing on an
entrance is why an animation feels sluggish even at a short duration.

**No bounce or spring in PRODUCT mode.** Overshoot on a settings toggle implies physical mass that a
checkbox does not have, and it delays the confirmation the person is waiting for.

---

## Reduced motion is mandatory

Above `MOTION_INTENSITY` 3, every animation branches. This is not an enhancement; it is a browser
setting a real person enabled, often for a vestibular condition where large motion causes nausea.

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

That global reset is the floor, not the goal. Better: keep the *state change* and drop the
*travel*. An opacity crossfade at 100ms communicates the change without moving anything.

```css
.reveal { opacity: 0; transform: translateY(1.5rem); transition: all 400ms var(--ease-out); }
@media (prefers-reduced-motion: reduce) {
  .reveal { transform: none; transition: opacity 150ms linear; }
}
```

Also honor `prefers-reduced-transparency` - any blur-and-translucency surface needs an opaque
fallback, or the effect becomes unreadable for the people who turned it off.

---

## Scroll-driven animation

Scroll effects are declarative now. They run off the main thread, which is why they are smooth.

```css
@keyframes reveal {
  from { opacity: 0; transform: translateY(2rem); }
  to   { opacity: 1; transform: none; }
}

.card {
  animation: reveal linear both;
  animation-timeline: view();        /* progresses as the element crosses the viewport */
  animation-range: entry 0% entry 60%;
}

.progress-bar {
  animation: grow linear both;
  animation-timeline: scroll();      /* progresses with scroll position of the nearest scroller */
}
```

`view()` is element-relative; `scroll()` is scroller-relative. `animation-range` controls which part
of the crossing maps to the animation.

**Never drive scroll animation from JavaScript state.**

```js
// WRONG - a state update per scroll event, so the tree re-renders at scroll frequency
window.addEventListener("scroll", () => setY(window.scrollY));
```

If a scroll effect genuinely needs JS, it writes to a motion value or a CSS custom property, never to
component state. And it needs cleanup on unmount, or it leaks across navigations.

The same rule applies to continuous pointer values: mouse position, drag offset, and physics never
live in component state.

---

## View transitions

```css
@view-transition { navigation: auto; }              /* cross-document, same origin */

.hero-image { view-transition-name: hero; }         /* must be UNIQUE per page */
```

```js
document.startViewTransition(() => updateTheDOM());  // same-document
```

**Pitfall that will bite:** `view-transition-name` must be unique in the document at transition time.
Applying one name to every item in a list silently kills the transition. Generate per-item names.

Wrap it in a capability check and skip the transition under reduced motion.

---

## Entry animation for elements that did not exist

```css
@starting-style {
  .popover:popover-open { opacity: 0; transform: scale(0.96); }
}
.popover:popover-open { opacity: 1; transform: scale(1); transition: all 200ms var(--ease-out); }
```

`@starting-style` provides the "before" state for an element entering the DOM or leaving
`display: none`. Without it, entry transitions on newly-displayed elements do nothing, and the usual
workaround is a `requestAnimationFrame` double-tick that is no longer needed.

Also allow the element to transition out:

```css
.popover { transition: opacity 200ms, transform 200ms, overlay 200ms allow-discrete,
                       display 200ms allow-discrete; }
```

---

## Stagger without JavaScript

```css
.list > * {
  animation: reveal 400ms var(--ease-out) both;
  animation-delay: calc((sibling-index() - 1) * 40ms);
}
```

Keep the total stagger under about 400ms. Beyond that the last item feels late, and the person has
already moved on.

---

## Perpetual motion

Anything that loops forever costs attention permanently and delivers information once.

- **At most one continuously-moving element per page**, MARKETING only.
- Never in PRODUCT mode. A dashboard that never stops moving cannot be read.
- A looping animation next to text makes the text harder to read. That is not a preference; peripheral
  motion pulls the eye involuntarily.

---

## Budget by dial

| `MOTION_INTENSITY` | What exists |
| --- | --- |
| 1-2 | State changes only. No transitions beyond instant focus and hover color. |
| 3 | Hover and focus transitions, 120-180ms. No entrance animation. |
| 4-6 | Add entrance transitions on interactive surfaces, one orchestrated page-level moment, panel and dropdown transitions. |
| 7-8 | Scroll-driven reveals, view transitions, one continuous element. |
| 9-10 | Pinned scroll sequences, layered parallax, pointer-reactive physics. MARKETING only, and only where the motion *is* the product. |

Reduced-motion branching becomes mandatory at 4.

At phase 4, read this table backwards: find the row that describes what the build actually does, and
that is the realized `MOTION_INTENSITY`. One count-up on load is row 4-6 only if the rest of that row
is also present; on its own it is row 3. Two or more below the declared value is signature `A7`.

---

## Motion claimed is motion shown

If the plan says an element is magnetic, or a panel slides, or a number counts up, the built output
must actually do it. A described interaction that was not implemented is the most expensive kind of
gap, because it is discovered by the person using it rather than by a review.

---

## Locks

Record in `DESIGN.md`: the duration set, the easing set, and an explicit list of what is allowed to
move. The list is a budget, so a later session adding a section cannot quietly double the motion.
