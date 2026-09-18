# Color

**Read at:** phase 1 (choosing), phase 2 (locking).

---

## Author in OKLCH

`oklch(L C H)` - lightness 0-1, chroma 0-0.4ish, hue 0-360.

Why it matters practically, not theoretically: in `hsl()`, two colors at the same stated lightness
look nothing alike. `hsl(60 100% 50%)` and `hsl(240 100% 50%)` are both "50% light"; one is yellow
that burns, one is blue that reads as near-black. So an `hsl` palette cannot be built by holding
lightness and walking hue, which is exactly what building a palette is.

OKLCH lightness is perceptual. Hold `L`, walk `H`, and every result has the same visual weight.
That single property is what makes a systematic palette possible.

```css
:root {
  --brand:  oklch(0.55 0.18 264);
  --accent: oklch(0.55 0.18 145);  /* same L and C, different hue - reads as equally strong */
}
```

Chroma is not uniform across hues at high lightness. A very light, very saturated yellow exists;
a very light, very saturated blue does not. If a value falls outside the display gamut the browser
clamps it, so check light tints visually rather than trusting the numbers.

---

## Three layers

Never skip a layer. Never reference a primitive from a component.

```css
/* 1. PRIMITIVE - raw values. Named for what they ARE. No meaning attached. */
:root {
  --blue-500: oklch(0.55 0.18 264);
  --blue-600: oklch(0.48 0.19 264);
  --gray-050: oklch(0.98 0.002 264);
  --gray-900: oklch(0.21 0.01 264);
}

/* 2. SEMANTIC - named for what they MEAN. This layer is the design system. */
:root {
  --color-brand:          var(--blue-500);
  --color-surface:        var(--gray-050);
  --color-text:           var(--gray-900);
  --color-text-muted:     oklch(from var(--color-text) l c h / 0.65);
  --color-border:         oklch(from var(--color-text) l c h / 0.12);
}

/* 3. COMPONENT - named for WHERE. Only when a component genuinely deviates. */
:root {
  --button-bg:      var(--color-brand);
  --button-bg-hover: color-mix(in oklch, var(--color-brand), black 12%);
}
```

**Call sites use layer 2, or layer 3 when it exists. Never layer 1.** A component that reaches for
`--blue-500` has hardcoded a decision that the semantic layer exists to own. When the brand changes
hue, layer 2 changes in one place and layer 1 becomes dead weight; a component touching layer 1
silently keeps the old brand.

**Grays carry a hue.** `oklch(0.98 0.002 264)` is a gray tinted toward the brand hue. Pure
`oklch(L 0 0)` grays next to a saturated brand read as dirty. Give every neutral a chroma of
0.002-0.015 at the brand's hue.

---

## Derive states, do not pick them

Hover, active, disabled, and subtle variants are **functions of** the base color, not new colors.
Picking them by hand produces a palette that drifts.

```css
.btn {
  background: var(--color-brand);
}
.btn:hover {
  /* darken toward black in perceptual space */
  background: color-mix(in oklch, var(--color-brand), black 12%);
}
.btn:active {
  background: color-mix(in oklch, var(--color-brand), black 20%);
}
.btn:disabled {
  /* drop chroma, keep lightness - reads as "inert" not "different color" */
  background: oklch(from var(--color-brand) l calc(c * 0.15) h);
}
```

Relative color syntax `oklch(from <color> L C H)` destructures an existing color so any channel can
be recomputed:

```css
--brand-tint:       oklch(from var(--brand) 0.96 calc(c * 0.12) h);  /* wash background */
--brand-complement: oklch(from var(--brand) l c calc(h + 180));
--brand-analogous:  oklch(from var(--brand) l c calc(h + 30));
```

One consequence worth stating: a whole palette can be generated from **one** brand value. That is
the correct amount of hand-picked color in a system.

---

## Both color modes from one declaration

```css
:root {
  --color-surface: light-dark(oklch(0.99 0.002 264), oklch(0.17 0.01 264));
  --color-text:    light-dark(oklch(0.21 0.01 264), oklch(0.96 0.003 264));
  color-scheme: light dark;   /* required, or light-dark() does nothing */
}
```

`color-scheme` is not optional - it is what tells `light-dark()` which branch is active, and it also
fixes native form controls and scrollbars in dark mode.

**Pick one mechanism.** `light-dark()`, or a `.dark` class overriding custom properties, or a media
query. Never two. Two mechanisms means every future color bug takes twice as long to locate.

Neither mode uses pure values. `#000` on an OLED panel makes adjacent text vibrate; `#fff` at night
is a flashlight. Floors and ceilings:

```
dark mode surface:  oklch(0.14 - 0.20)   not 0
light mode surface: oklch(0.96 - 0.99)   not 1
```

---

## Contrast, with the actual numbers

| Content | Minimum ratio |
| --- | --- |
| Body text, and any text under 18.66px, or under 24px if bold | **4.5:1** |
| Large text (24px+, or 18.66px+ bold) | **3:1** |
| Icons and graphics required to understand content | **3:1** |
| Component boundaries needed to identify a control (input borders) | **3:1** |
| Focus indicator against the adjacent background | **3:1** |
| Purely decorative, and disabled controls | exempt |

Two failures happen constantly and both are invisible until measured:

1. **Text on the brand color.** A mid-lightness brand often fails against both white and near-black
   text. Check it. If it fails both, the brand is not usable as a button fill at that lightness -
   change the lightness, do not shrink the text.
2. **Placeholder and muted text.** `opacity: 0.5` on body text lands near 2.5:1. Muted text needs
   its own token that was checked, not an opacity applied to a token that passed.

---

## Palette size

**One accent. Maximum.** Chroma above roughly 0.20 starts to feel synthetic in UI; keep the accent
below it unless the brief is explicitly loud.

A complete palette:

```
1  surface        the page
2  surface-raised one step up, for grouped content
3  text           primary
4  text-muted     secondary
5  border         hairlines and control edges
6  brand          the one accent
+  status         success / warning / danger / info, only if the product has states
```

Six values plus status. If a palette has eleven, it is compensating for weak hierarchy in type and
space. Fix the hierarchy instead.

Status colors are **exempt** from the one-accent rule, and only when encoding actual state. Apply
them to the value itself, never to a whole row background.

---

## Banned clusters

These are not ugly. They are **defaults** - they appear regardless of subject, which is what makes
them a tell. If the brief explicitly asks for one, the brief wins. If the brief leaves the axis
free, do not spend that freedom here.

**The synthetic gradient.** A violet-to-magenta or blue-to-violet linear gradient, typically at
135deg, most often around `#8b5cf6` to `#ec4899` or `#3b82f6` to `#8b5cf6`. The single most
recognizable generated-design signature that exists. Severity 5.

**The default blue-violet pair.** `#3b82f6` alongside `#8b5cf6` as brand and accent. These are the
mid-steps of the most common utility-framework palette, shipped unchanged.

**The cream-and-clay cluster.** A warm off-white background in the `#f5f1ea` / `#f7f5f1` /
`#fbf8f1` / `#efeae0` / `#ece6db` family, a high-contrast serif display face, and a terracotta or
warm-clay accent near `#d97757` / `#b6553a` / `#b08947`. Reads as premium exactly once; it is now
a template. Text in this cluster is usually `#1a1714` / `#1b1814`.

**Near-black plus one acid.** A `#0b0b0b` or `#111` background with a single bright acid-green or
vermilion accent.

**Tinted near-black standing in for black.** `#0b0b0b`, `#111`, `#0f0f0f` used as "black". If the
design wants black, the neutral scale should already provide it at the brand hue.

---

## Alternatives that are not those

Descriptions, not prescriptions. Each is a *relationship*, so it survives a hue change.

| Direction | Structure |
| --- | --- |
| **Cold luxury** | near-black surface, one cold light neutral, chroma under 0.04 everywhere, hierarchy entirely from lightness |
| **Single deep hue** | one dark saturated hue as surface (deep forest, deep navy, deep aubergine), text as a light tint of the *same* hue, accent as that hue at high lightness |
| **Paper and ink** | warm light surface, true dark text, exactly one saturated accent used under five times per page |
| **Two-temperature** | one cool structural hue and one warm accent, roughly 150-210deg apart, neither exceeding chroma 0.16 |
| **Monochrome plus interrupt** | full grayscale system, one saturated hue reserved for a single interrupt state and used nowhere else |
| **Earth triad** | three desaturated earth hues at distinctly different lightness levels, the darkest carrying text |

Before committing, name the palette's *rule* in one sentence. If the rule cannot be stated, there is
no palette, only six colors that were chosen one at a time.

---

## Locks

Once `DESIGN.md` records the palette:

- **One palette per project.** No section introduces its own hues.
- **No theme inversion per section** unless the brief explicitly wants a deliberate switch, and then
  once, not per section.
- **Every new color goes through the semantic layer.** A component that needs a color the semantic
  layer lacks means the layer is incomplete - extend it in `DESIGN.md`, do not inline the value.
- **Never encode meaning in color alone.** Every state also carries text, an icon, a border, or a
  position change. Color-only state is invisible to a meaningful share of users and to anyone on a
  bad screen in sunlight.
