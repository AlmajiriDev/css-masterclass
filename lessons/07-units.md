# Lesson 07 — Units & Values

> **The measuring-tape analogy.** A carpenter has a tape measure that switches between inches, centimeters, and "hand widths" depending on the job. CSS units are the same — `px` for fixed, `em`/`rem` for relative, `vw`/`vh` for screen size, `%` for parent-based.

## What you'll learn

- The most common units: `px`, `em`, `rem`, `%`, `vw`, `vh`, `fr`.
- When to use which.
- How `calc()` lets you mix units with math.
- Values that aren't numbers: keywords, colors.

## Why this matters

Choosing the wrong unit makes designs rigid on phones, weirdly large on big screens, or impossible to scale for accessibility. Picking the right unit is the difference between a design that works *everywhere* and one that breaks.

## The major units

### Absolute units — fixed size

```css
.box { width: 200px; }   /* pixels */
```

| Unit | Notes |
|------|-------|
| `px`  | The workhorse. A pixel is the smallest dot your screen can draw (effectively). Use for borders, shadows, fine details. |

Other absolute units (`cm`, `mm`, `in`, `pt`) exist but you will **never** need them for screen design.

### Relative units — relative to something

```css
.box { font-size: 1.2em; }       /* 1.2 × parent's font-size */
.box { font-size: 1.2rem; }      /* 1.2 × root font-size */
.box { width: 50%; }              /* 50% of parent's width */
```

| Unit | Relative to | When to use |
|------|-------------|-------------|
| `em`   | Parent's `font-size` | Inner component sizes that should scale with nearby text |
| `rem`  | Root `<html>` font-size | Most typography and gaps — picks up user accessibility settings |
| `%`    | Parent's height or width | Width of layouts, image scaling |
| `vw`   | 1% of viewport's width | "Always fit the screen width" elements |
| `vh`   | 1% of viewport's height | Full-screen sections |
| `vmin` / `vmax` | smaller / larger of width/height | Edge cases like "never get taller than the screen" |
| `fr`   | Grid leftover space | Grid columns/rows |

### The root font-size default

Browsers default to `16px` for `<html>`. So `1rem = 16px`. If the user sets browser zoom to 200%, `1rem = 32px`. That's why rems are **accessibility-friendly**.

### `calc()` — math at runtime

```css
.header {
  height: calc(100vh - 80px);   /* viewport minus a fixed bar */
  width: calc(100% - 40px);
  font-size: calc(1rem + 0.5vw);   /* grows with screen */
}
```

You can add, subtract, multiply, divide units together. `calc(50% - 10px)` works. So does `calc(8px + 1rem)`. The value is computed when the page renders.

### Modern helpers (use these)

```css
width: min(90%, 1200px);     /* the smaller of 90% or 1200px */
gap:   max(8px, 1vw);        /* the larger of 8px or 1vw */
font-size: clamp(1rem, 2vw, 2rem);   /* never below 1rem, never above 2rem */
```

`min()`, `max()`, `clamp()` give you fluid, bounded sizes without media queries. We'll cover them deeply in Lesson 45.

## Number values without units

A few properties take **plain numbers**:

```css
line-height: 1.5;     /* multiplier, no unit */
opacity: 0.7;          /* 0 to 1 */
flex-grow: 2;          /* ratio */
z-index: 5;            /* integer */
```

## Special keyword values

Sometimes a property accepts a keyword instead of a number:

```css
display: block;        /* not "block;"  — keywords are unquoted */
text-align: center;
overflow: hidden;
cursor: pointer;
```

Some keywords can also be unquoted names: `font-family: "Helvetica Neue", sans-serif;` — the family name may be quoted but `sans-serif` (a generic family) is unquoted.

## Try it yourself

Create `practice/lesson07.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 07 — units</title>
  <style>
    :root { font-size: 16px; }
    body { font-family: system-ui, sans-serif; margin: 0; padding: 30px; background: #fffbf5; }

    .label { color: #555; font-size: 12px; margin-top: 8px; }

    .row { display: flex; gap: 12px; align-items: flex-end; flex-wrap: wrap; margin-bottom: 20px; }

    .px  { width: 100px;  height: 50px; background: #6d28d9; color: white;
           display: flex; align-items: center; justify-content: center; border-radius: 8px; }
    .pct { width: 30%;    height: 50px; background: #f97316; color: white;
           display: flex; align-items: center; justify-content: center; border-radius: 8px; }
    .rem { font-size: 1.5rem; padding: 1rem 1.5rem;
           background: #fef3c7; border-radius: 8px; }
    .em  { font-size: 1.5rem; }
    .em .inner { padding: 1em 1.5em; background: #ddd6fe; border-radius: 8px; }

    .calc { width: 80vw; height: 60px; background: #059669; color: white;
            display: flex; align-items: center; justify-content: center;
            border-radius: 8px; max-width: 100%; }
  </style>
</head>
<body>
  <h1>Units in action</h1>

  <div class="row">
    <div>
      <div class="px">100px</div>
      <div class="label">fixed pixels</div>
    </div>
    <div>
      <div class="pct">30%</div>
      <div class="label">percent of parent</div>
    </div>
  </div>

  <div class="row">
    <div class="rem">1.5rem text (24px)</div>
  </div>

  <div class="em">
    outer (1.5rem)
    <div class="inner">inner (1em padding)</div>
  </div>

  <div class="calc">80vw wide (responsive)</div>

  <p style="margin-top: 30px;">
    Resize the window and watch the percentage, viewport, and rems change —
    while the 100px square stays exactly 100px.
  </p>
</body>
</html>
```

Resize the window and watch.

## Common mistakes

- **Using `px` for everything** — the site will feel rigid on mobile. Use `rem` for text and `%` or `vw` for widths.
- **`em` chains** — `em` inside `em` inside `em` multiplies. A `font-size: 2em` inside a `2em` parent is 4em. Use `rem` to avoid surprises.
- **Forgetting spaces in `calc`** — `calc(100%-40px)` is invalid; `calc(100% - 40px)` is fine.
- **Mixing `vw` for text and forgetting mobile** — 10vw text is fine on desktop, huge on a phone. Always pair with `min`/`max`/`clamp`.

## Debugging tips

- In DevTools → Computed tab, every value shows the unit it finally resolved to. Click to see if `1.2rem` became `19.2px`.

## Quick quiz

1. What's the default root `font-size`?
2. What's the difference between `em` and `rem`?
3. Write a `width` that takes the smaller of 800px or 90% of its parent.

<details>
<summary>Answers</summary>

1. `16px`.
2. `em` = relative to **parent** font-size. `rem` = relative to **root** font-size.
3. `width: min(800px, 90%);`

</details>

## Mini challenge

Make a hero section that is **exactly** full-screen height minus a `70px` header. Use `calc()`. Confirm by resizing the window.

## What's next

We can pick units. Now the most important CSS rule you'll ever meet — **who wins when rules clash** — the cascade.

→ [Lesson 08 — Cascade, Inheritance & Specificity](08-cascade.md)
