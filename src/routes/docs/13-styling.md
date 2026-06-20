---
title: Styling
id: 13
group: customization
description: "currentColor inheritance, the class prop, and styling the wrapper."
---

Icons inherit color from their parent's `color` and accept extra CSS through the `class` prop.

## `currentColor` inheritance

Every icon's strokes and fills use `currentColor`. That means a single `color` declaration on a parent - or the icon itself - is enough to recolor it.

```svelte
<button style="color: tomato;">
  <Heart size={20} />
  Like
</button>

<button style="color: var(--brand-primary);">
  <Gear size={20} />
  Settings
</button>
```

No `fill=` or `stroke=` props to remember; no per-icon override mechanism to manage.

## The `class` prop

Pass extra class names onto the wrapper `<div class="animated-icon …">`:

```svelte
<Gear class="toolbar-icon" />
<Heart class="favorite-icon is-active" />
```

Use it to:

- Set a CSS size (see [Size](/docs/11-size)).
- Apply hover / focus / active states via parent selectors.
- Target icons in tests.

The wrapper itself is unstyled by default - `display`, `width`, `height`, etc. all flow from your CSS or the inline `size` prop.

## Targeting the inner `<svg>`

The inner SVG has `style="overflow: visible; display: block;"` baked in. To restyle from CSS without using a class on the wrapper:

```css
:global(.animated-icon > svg) {
  filter: drop-shadow(0 1px 2px rgba(0, 0, 0, 0.1));
}
```

Inside a Svelte component, drop `:global()` only if you're styling across slots. From a SASS file, scope as you normally would.

## Hover and focus states

```css
.btn-icon {
  transition: transform 120ms ease-out;
}
.btn-icon:hover {
  transform: translateY(-1px);
}
```

```svelte
<button class="btn">
  <Gear class="btn-icon" template="spin" />
  Settings
</button>
```

The icon's own animation (`template="spin"`) runs on its own internal hover. The wrapper's hover style runs on the button's. They compose without fighting.

## Dark mode and themes

Because the icon color is just `currentColor`, theme switching is free:

```svelte
<div class="root" style="color: var(--icon-color);">
  <Heart />
</div>
```

```css
:root { --icon-color: #111; }
:root[data-theme='dark'] { --icon-color: #f5f5f5; }
```

## Inline SVG (no wrapper)

If you need to embed an icon inline - for example, inside a CSS `mask-image`, or to control its position absolutely - use the raw SVG and skip the animation engine. The codegen-emitted icons aren't built for that, but you can copy the inner `<path>` markup from any generated file and use it directly.

For a fully styled animated icon, stick with the wrapper - it's what holds the mouse handlers and the imperative control surface.