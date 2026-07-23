# Lesson 44 — Media Queries

> **The thermometer analogy.** Media queries are like a thermometer — when the temperature (screen width) crosses a threshold, the CSS changes clothes. "When the device is small, do X. When big, do Y."

## What you'll learn

- The full `@media` syntax.
- Width, height, orientation, color scheme, hover, and pointer queries.
- Common breakpoints.
- Container queries (the modern sibling).

## Why this matters

`@media` is how responsive design actually changes layout. Knowing what queries are available lets you write better conditions, not just `min-width`.

## The basic syntax

```css
@media (min-width: 700px) {
  /* styles for screens at least 700px wide */
}
@media (max-width: 699px) {
  /* styles for screens narrower than 700px */
}
@media (min-width: 700px) and (max-width: 1000px) {
  /* styles for 700 - 1000px */
}
```

The condition uses normal CSS values and is enclosed in `(...)`. Multiple conditions use `and`, `or`, `not`.

## Mobile-first (always)

Write base styles for **mobile**, then add `min-width` queries:

```css
.grid { grid-template-columns: 1fr; }

@media (min-width: 640px)  { .grid { grid-template-columns: repeat(2, 1fr); } }
@media (min-width: 1024px) { .grid { grid-template-columns: repeat(4, 1fr); } }
```

This is cleaner than `max-width` cascades.

## Common breakpoints (Tailwind-inspired)

| Token | Min width | Targets |
|-------|-----------|---------|
| (default) | 0 | mobile portrait |
| sm | 640px | small tablet / large phone landscape |
| md | 768px | tablet portrait |
| lg | 1024px | laptop |
| xl | 1280px | desktop |
| 2xl | 1536px | large desktop |

Pick your own. Just keep them consistent.

## Other useful media features

```css
@media (orientation: landscape) { /* wider than tall */ }
@media (orientation: portrait)  { /* taller than wide */ }
@media (prefers-color-scheme: dark) {
  body { background: #1e1b3a; color: white; }
}
@media (prefers-reduced-motion: reduce) {
  * { animation-duration: 0.01ms !important; }
}
@media (hover: hover) {
  /* device has a hover-capable pointer (mouse) */
  .btn:hover { background: orange; }
}
@media (pointer: coarse) {
  /* a finger — not a mouse */
  .btn { padding: 14px 24px; }    /* bigger touch targets */
}
@media (print) {
  /* printed page styles */
  .no-print { display: none; }
}
```

## `min-resolution` for high-DPI displays

```css
@media (-webkit-min-device-pixel-ratio: 2),
       (min-resolution: 2dppx) {
  /* retina display tweak */
}
```

Usually you don't need this — image formats and SVG handle it.

## Container queries — modern sibling

Container queries respond to the **size of the parent**, not the viewport. Great for components.

```html
<div class="card-container">
  <article class="card">…</article>
</div>
```

```css
.card-container {
  container-type: inline-size;
  container-name: card;
}

.card { padding: 12px; }

/* when container is wide, restructure the card */
@container card (min-width: 400px) {
  .card {
    padding: 24px;
    display: grid;
    grid-template-columns: 1fr 2fr;
    gap: 16px;
  }
}
```

This pattern lets a card behave differently inside a narrow sidebar than in a wide main area.

## Range syntax (newer)

```css
@media (width >= 700px) { /* same as min-width: 700px */ }
@media (width <= 1000px) { /* max-width: 1000px */ }
@media (700px <= width <= 1000px) { /* a range */ }
```

Modern browsers support this. Older parsers may not — prefer the regular syntax if you must support legacy browsers.

## Try it yourself

Create `practice/lesson44.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Lesson 44 — media queries</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body {
      font-family: system-ui, sans-serif;
      margin: 0;
      background: #fffbf5;
      color: #1e293b;
    }
    .container { width: min(90%, 1100px); margin: 0 auto; padding: 24px; }

    h1 { color: #6d28d9; }

    .grid {
      display: grid;
      gap: 16px;
      grid-template-columns: 1fr;
    }
    @media (min-width: 600px) {
      .grid { grid-template-columns: repeat(2, 1fr); }
    }
    @media (min-width: 900px) {
      .grid { grid-template-columns: repeat(3, 1fr); }
    }
    .card {
      background: white;
      padding: 16px;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.08);
    }

    /* dark-mode styles */
    @media (prefers-color-scheme: dark) {
      body { background: #0f0f1a; color: #f1f5f9; }
      .card { background: #1e1b3a; color: white; }
      h1   { color: #fde68a; }
    }

    /* container query */
    .wrap {
      container-type: inline-size;
      container-name: profile;
    }
    .profile {
      background: white;
      border-radius: 12px;
      padding: 12px;
      display: grid;
      gap: 12px;
      grid-template-columns: 1fr;
    }
    @container profile (min-width: 300px) {
      .profile { grid-template-columns: 80px 1fr; align-items: center; }
    }
    @media (prefers-color-scheme: dark) {
      .profile { background: #1e1b3a; }
    }
    .profile .pic {
      width: 60px; height: 60px;
      background: linear-gradient(135deg,#6d28d9,#f97316);
      border-radius: 50%;
    }
    @container profile (min-width: 300px) {
      .profile .pic { width: 80px; height: 80px; }
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Media &amp; container queries</h1>
    <p style="color:#6b7280">Resize the browser or toggle dark mode in DevTools.</p>

    <div class="grid">
      <div class="card"><h3>Card 1</h3>1 col on mobile.</div>
      <div class="card"><h3>Card 2</h3>2 cols from 600 px.</div>
      <div class="card"><h3>Card 3</h3>3 cols from 900 px.</div>
    </div>

    <h2>Container query — profile card</h2>
    <div class="wrap" style="background:#fef3c7; padding: 12px; border-radius:12px;">
      <div class="profile">
        <div class="pic"></div>
        <div>
          <strong>Hauwa</strong>
          <p style="margin: 4px 0; color:#6b7280">Designer &amp; CSS fan</p>
        </div>
      </div>
    </div>

    <p style="color:#6b7280">If you nest the wrap in a 200px sidebar, the card stacks. In a 400px main area, it goes horizontal.</p>
  </div>
</body>
</html>
```

In DevTools, toggle dark mode (Rendering panel → "Emulate CSS prefers-color-scheme: dark") to see the color flip.

## Common mistakes

- **Using `max-width`** — works, but mobile-first with `min-width` is cleaner.
- **Too many breakpoints** — pick 3 (mobile / tablet / desktop).
- **No `prefers-reduced-motion`** — motion-sick users suffer.

## Quick quiz

1. Write a `@media` rule for screens at least 768 px wide.
2. What's the difference between media and container queries?
3. Which media feature detects dark mode preference?

<details>
<summary>Answers</summary>

1. `@media (min-width: 768px) { ... }`
2. Media queries = viewport size. Container queries = parent element size.
3. `prefers-color-scheme: dark`.

</details>

## Mini challenge

Build a "responsive article" — heading + content + sidebar — where the sidebar stacks below on mobile and floats to the right on desktop (≥900 px).

## What's next

Now we look at the math functions that make responsive sizing clean.

→ [Lesson 45 — Math Functions: calc, min, max, clamp](45-math-functions.md)
