---
title: Size
id: 11
group: customization
description: "Sizing icons and the fixed 256 viewBox."
---

The `size` prop sets the rendered pixel size of the icon. Default is `24`.

```svelte
<Gear size={16} />
<Gear size={24} />
<Gear size={32} />
<Gear size={48} />
<Gear size={96} />
```

`size` controls both `width` and `height` of the inner `<svg>`.

## The fixed 256 viewBox

Every icon ships with `viewBox="0 0 256 256"`, baked in by the codegen pipeline. That uniformity is what lets a single animation template move any icon correctly - the geometry is always on the same scale, regardless of the icon's source.

You don't need to think about it. The `size` prop just sets the **render** size; the viewBox handles the math.

## Sizing via CSS

If you'd rather size icons with CSS than props, drop `size` and let the `class` prop target the wrapper:

```svelte
<style>
  .icon { width: 1.5rem; height: 1.5rem; }
</style>

<Gear class="icon" />
```

The `class` prop applies to `<div class="animated-icon …">`. The inner `<svg>` already has `style="overflow: visible; display: block;"`, so the wrapper's dimensions drive the visible size.

> The library uses SASS for the docs site; in your own app, use whatever styling system fits.

## Inline size

`<svg width="…" height="…">` from the `size` prop wins over CSS unless you use `!important`. To make icons fluid inside a flex layout:

```css
.icon { width: 100%; height: auto; }
```

and override the inline attributes:

```svelte
<Gear class="icon" size={undefined} />
```

Or use `AnimatedIcon` directly and skip the `size` prop entirely - pass `width` and `height` via the surrounding styles.

## Common sizes

| px | When |
|---|---|
| 12 – 16 | Inline with body text, table cells, dense lists |
| 20 – 24 | Default UI, buttons, nav, toolbars |
| 32 – 48 | Section headers, empty states, feature cards |
| 64 – 96 | Hero / splash moments |
| 128+ | Decorative or showcase (use sparingly) |

`size={24}` is the default and matches the most common UI icon scale.