# Lesson 45 — Math Functions: calc, min, max, clamp

> **The ruler-but-better analogy.** CSS has a built-in calculator. `calc()` lets you mix units. `min()`, `max()`, and `clamp()` pick the most sensible value at the moment. Together they replace 90% of your media queries.

## What you'll learn

- `calc()` — any arithmetic on any units.
- `min()`, `max()` — choose the smallest/largest.
- `clamp(min, preferred, max)` — bound a fluid value.
- A few real-world patterns.

## Why this matters

Most "responsive" sizes don't need media queries. They can be expressed as math. Less code, fewer breakpoints, smoother transitions.

## `calc()`

```css
width: calc(100% - 40px);    /* subtract constant */
font-size: calc(1rem + 0.5vw);   /* mix units */
height: calc(100vh - 64px);  /* viewport minus header */
padding: calc(1rem + 4px);   /* unusual but valid */
```

You can do `+`, `-`, `*`, `/`. Spaces around `+` and `-` matter, not around `*` and `/`.

```css
.grid {
  width: calc((100% - 40px) / 3);     /* 3 columns with 20px gaps */
}
```

Useful for old browsers, but `gap` does most of what we want.

## `min()` — pick the smaller

```css
width: min(90%, 1200px);     /* never wider than 1200px */
```

If 90% of parent is 800px → use 800px.
If 90% of parent is 1500px → use 1200px (the smaller).

## `max()` — pick the larger

```css
min-height: max(300px, 50vh);  /* at least 50vh, but never smaller than 300px */
font-size: max(16px, 1rem);    /* never go below 16px even if rem shrinks */
```

## `clamp(min, preferred, max)` — bound a fluid value

```css
font-size: clamp(1rem, 1.5vw, 2rem);
/*               ↑min   ↑fluid   ↑max */
```

- Smaller than 1rem → 1rem.
- Bigger than 2rem → 2rem.
- Otherwise → 1.5vw (grows linearly).

### A common typography scale

```css
:root {
  --step--1: clamp(0.94rem, 0.94rem + 0.1vw, 1rem);
  --step-0:  clamp(1.13rem, 1.05rem + 0.4vw, 1.31rem);
  --step-1:  clamp(1.35rem, 1.20rem + 0.75vw, 1.77rem);
  --step-2:  clamp(1.62rem, 1.36rem + 1.3vw, 2.38rem);
  --step-3:  clamp(1.94rem, 1.54rem + 2vw, 3.16rem);
}
body { font-size: var(--step-0); }
h1   { font-size: var(--step-3); }
```

This is the "fluid type scale" used by modern design systems. Smooth between breakpoints, capped at extremes.

### Clamped button

```css
.btn {
  padding: clamp(0.5rem, 1.2vw, 1rem) clamp(1rem, 3vw, 2rem);
  font-size: clamp(0.875rem, 0.5vw + 0.75rem, 1rem);
}
```

The button feels right on every screen.

## Real patterns

### Hero section, viewport-aware

```css
.hero {
  min-height: clamp(400px, 70vh, 800px);
  padding: clamp(2rem, 6vw, 6rem);
}
```

### Card width

```css
.card {
  width: min(100%, 320px);    /* never wider than 320, never wider than parent */
}
```

### Gap between sections

```css
.section + .section {
  margin-top: clamp(2rem, 5vw, 5rem);
}
```

### Sidebar / main adaptive

```css
.layout {
  display: grid;
  grid-template-columns: minmax(0, 240px) minmax(0, 1fr);
}
```

## Try it yourself

Create `practice/lesson45.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Lesson 45 — math functions</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body { font-family: system-ui, sans-serif; padding: 0; margin: 0; background: #fffbf5; color: #1e293b; }
    section { padding: clamp(1rem, 4vw, 4rem); max-width: 1100px; margin: 0 auto; }
    h1 { color: #6d28d9; }

    /* fluid type */
    .big {
      font-size: clamp(2rem, 8vw, 6rem);
      line-height: 1.05;
      letter-spacing: -0.04em;
      text-align: center;
      margin: 24px 0;
    }
    p { font-size: clamp(1rem, 1.5vw, 1.2rem); line-height: 1.6; }

    .grid {
      display: grid;
      gap: 16px;
      grid-template-columns: repeat(auto-fit, minmax(clamp(220px, 90%, 320px), 1fr));
      margin: 24px 0;
    }
    .card {
      background: white;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.08);
    }

    /* min() vs fixed */
    .box1 {
      width: min(90%, 600px);
      background: #ede9fe;
      padding: 20px;
      border-radius: 8px;
      margin: 24px auto;
      text-align: center;
    }
    .box2 {
      width: 90%;
      max-width: 600px;     /* traditional alternative to min() */
      background: #d1fae5;
      padding: 20px;
      border-radius: 8px;
      margin: 0 auto;
      text-align: center;
    }

    /* fluid button */
    .btn {
      display: inline-block;
      padding: clamp(0.5rem, 1.2vw, 1rem) clamp(1rem, 3vw, 2rem);
      font-size: clamp(0.875rem, 0.5vw + 0.75rem, 1rem);
      background: #6d28d9;
      color: white;
      border: 0;
      border-radius: 8px;
      cursor: pointer;
    }

    .hero {
      min-height: clamp(400px, 70vh, 800px);
      padding: clamp(2rem, 6vw, 6rem);
      background: linear-gradient(135deg, #6d28d9, #f97316);
      color: white;
      display: grid; place-items: center;
      text-align: center;
    }
    .hero h1 {
      color: white;
      font-size: clamp(2rem, 6vw, 4.5rem);
      margin: 0;
      line-height: 1.1;
    }
  </style>
</head>
<body>
  <section>
    <h1>Math functions</h1>
    <p>This whole page has <em>no media queries</em> for size adjustments — only <code>clamp()</code>, <code>min()</code>, and <code>max()</code>.</p>
  </section>

  <div class="hero">
    <h1>Resize me<br>I'm fluid</h1>
  </div>

  <section>
    <h2>Fluid type</h2>
    <div class="big">I scale from 32px to 96px smoothly.</div>

    <h2>min() vs the old way</h2>
    <div class="box1">width: min(90%, 600px)</div>
    <div class="box2">width: 90%; max-width: 600px (same look)</div>

    <h2>Fluid button</h2>
    <button class="btn">Click me</button>

    <h2>Cards that fit</h2>
    <div class="grid">
      <div class="card"><h3>Card 1</h3><p>Resizes &amp; reflows with viewport.</p></div>
      <div class="card"><h3>Card 2</h3><p>Even at 1500px wide.</p></div>
      <div class="card"><h3>Card 3</h3><p>Each card 220-320 wide.</p></div>
    </div>
  </section>
</body>
</html>
```

Resize the window — everything adjusts smoothly.

## Common mistakes

- **Math with no space** — `calc(100%-40px)` is invalid; needs `calc(100% - 40px)`.
- **Mixing % and px incorrectly** — adding them is fine, multiplying isn't (need to convert to common unit).
- **Forgetting min/max bounds** — `clamp(1rem, 10vw, 4rem)` lets the value get enormous at huge viewports.

## Quick quiz

1. Make a fluid font size that grows with the viewport but never exceeds 2.5rem.
2. Make a width that is at most 800px but never wider than 90% of the parent.
3. Use `calc()` to make a 100%-of-parent-minus-2rem width.

<details>
<summary>Answers</summary>

1. `font-size: clamp(1rem, 4vw, 2.5rem);`
2. `width: min(90%, 800px);`
3. `width: calc(100% - 2rem);`

</details>

## Mini challenge

Build a "pricing page" hero with a title and subtitle, both fluid-sized, and a CTA button that grows with the viewport (without media queries).

## What's next

Numbers and math, but the content is media — images, video, fonts.

→ [Lesson 46 — Responsive Images, Video & Container Queries](46-responsive-media.md)
