# Lesson 38 — Flexbox Fundamentals

> **The bookshelf analogy.** Default flow puts books on top of each other vertically. Flexbox lets you choose: books lined up side by side, by row or by column, with gaps and alignment. It's like a magic shelf that knows exactly where to put each item.

## What you'll learn

- How to turn an element into a flex container.
- The two axes: main vs cross.
- The 6 most-used container properties.
- When to choose Flexbox over Grid.

## Why this matters

Flexbox is the muscle you reach for daily. Navbars, buttons in toolbars, vertical-centered boxes, equal-height columns — Flexbox does it all.

## The basics

```html
<div class="row">
  <div>1</div>
  <div>2</div>
  <div>3</div>
</div>
```

```css
.row {
  display: flex;        /* turn on the bookshelf magic */
  gap: 16px;            /* space between items */
}
```

Just three lines and your items are arranged in a row. Replace `display: flex` with `display: inline-flex` to make it shrink to content.

## The two axes

- **Main axis** is the direction items are placed (`flex-direction`).
- **Cross axis** is perpendicular to it.

```
flex-direction: row     →  main axis: horizontal, cross: vertical
flex-direction: column  →  main axis: vertical,   cross: horizontal
```

## The 6 most-used container properties

### `flex-direction`

```css
.row { flex-direction: row; }            /* default */
.col { flex-direction: column; }
```

Also `row-reverse` and `column-reverse` for reversed order.

### `justify-content` (alignment on the main axis)

```css
.start    { justify-content: flex-start; }    /* default */
.end      { justify-content: flex-end; }
.center   { justify-content: center; }
.between  { justify-content: space-between; } /* space between, no space at edges */
.around   { justify-content: space-around; }  /* half-space at edges */
.evenly   { justify-content: space-evenly; }  /* equal space everywhere */
```

### `align-items` (alignment on the cross axis)

```css
.align-stretch    { align-items: stretch; }    /* default — fill cross axis */
.align-start      { align-items: flex-start; }
.align-end        { align-items: flex-end; }
.align-center     { align-items: center; }     /* vertically center items in a row! */
.align-baseline   { align-items: baseline; }   /* line up by text baseline */
```

### `flex-wrap`

```css
.nowrap   { flex-wrap: nowrap; }     /* default — all on one line */
.wrap     { flex-wrap: wrap; }       /* overflow to next line */
.wrap-rev { flex-wrap: wrap-reverse; }
```

### `gap`

```css
.row { gap: 16px; }                /* both axes */
.row { column-gap: 12px; row-gap: 20px; }
```

Replaces a dozen `margin-left` hacks. Modern and tidy.

### `align-content` (alignment of multiple lines, only when wrapped)

```css
.row { flex-wrap: wrap; align-content: space-between; }
```

Only matters if you have multiple lines.

## The shortcut for centered content

```css
.row {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 200px;
}
```

Three lines. Children perfectly centered.

## Visual aid

Imagine a row with 3 children:

```
[ A ]   [ B ]   [ C ]      ← justify-content: flex-start (default)

   [ A ]   [ B ]   [ C ]   ← justify-content: center

[ A ]      [ B ]      [ C ]    ← justify-content: space-between

[--A--][---B---][---C--]   ← justify-content: space-around

[--A--][--B--][--C--]   ← justify-content: space-evenly
```

## When to use Flexbox vs Grid

- **Flexbox** — 1-dimensional layouts (a row, a column). When items have different sizes and you want them to flow.
- **Grid** — 2-dimensional layouts (rows AND columns). When you have a structured grid of equal cells.

We'll cover Grid in Lesson 40.

## Try it yourself

Create `practice/lesson38.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 38 — flexbox</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body { font-family: system-ui, sans-serif; padding: 30px; background: #fffbf5; }
    h2 { color: #1e293b; margin-top: 30px; }

    .row {
      display: flex;
      gap: 12px;
      padding: 16px;
      background: white;
      border-radius: 12px;
      box-shadow: 0 4px 14px rgba(0,0,0,0.06);
      margin-bottom: 16px;
    }

    .box {
      background: #ede9fe;
      padding: 16px 20px;
      border-radius: 8px;
      font-weight: 700;
    }

    /* demonstrate justifications */
    .start   { justify-content: flex-start; }
    .end     { justify-content: flex-end; }
    .center  { justify-content: center; }
    .between { justify-content: space-between; }
    .around  { justify-content: space-around; }
    .evenly  { justify-content: space-evenly; }

    /* vertical center */
    .vcenter {
      display: flex;
      align-items: center;
      height: 100px;
    }

    /* different heights — note stretch vs center */
    .tall {
      align-items: stretch;
    }
    .tall .box:nth-child(2) { padding: 32px 20px; }
    .tall .box:nth-child(3) { padding:  8px 20px; }

    /* wrapping */
    .wrap-row {
      flex-wrap: wrap;
    }

    /* column direction */
    .column {
      display: flex;
      flex-direction: column;
      align-items: center;
      width: 200px;
    }
  </style>
</head>
<body>
  <h1>Flexbox</h1>

  <h2>justify-content (main axis)</h2>
  <div class="row start"><div class="box">A</div><div class="box">B</div><div class="box">C</div></div>
  <div class="row end"><div class="box">A</div><div class="box">B</div><div class="box">C</div></div>
  <div class="row center"><div class="box">A</div><div class="box">B</div><div class="box">C</div></div>
  <div class="row between"><div class="box">A</div><div class="box">B</div><div class="box">C</div></div>
  <div class="row around"><div class="box">A</div><div class="box">B</div><div class="box">C</div></div>
  <div class="row evenly"><div class="box">A</div><div class="box">B</div><div class="box">C</div></div>

  <h2>align-items (cross axis — note the 2nd and 3rd box are different heights)</h2>
  <div class="row tall">
    <div class="box">A</div>
    <div class="box" style="background:#fde68a">B (taller)</div>
    <div class="box" style="background:#d1fae5">C (shorter)</div>
  </div>

  <h2>Vertical center (3 lines)</h2>
  <div class="row vcenter" style="background:#f1f5f9">
    <div class="box">vertically centered</div>
  </div>

  <h2>Wrapping (flex-wrap)</h2>
  <div class="row wrap-row" style="max-width: 350px">
    <div class="box">1</div><div class="box">2</div><div class="box">3</div>
    <div class="box">4</div><div class="box">5</div><div class="box">6</div>
    <div class="box">7</div><div class="box">8</div>
  </div>

  <h2>Column direction</h2>
  <div class="column">
    <div class="box">One</div>
    <div class="box">Two</div>
    <div class="box">Three</div>
  </div>
</body>
</html>
```

## Common mistakes

- **Forgetting `display: flex`** — selectors still apply but they don't lay out as flex.
- **Mixing `align-items` with explicit `height`** — they're siblings, both work.
- **Wrapping with `margin` instead of `gap`** — `gap` is cleaner.

## Quick quiz

1. Center 3 boxes horizontally and vertically with the fewest lines.
2. Distribute them so the first is on the left, the last is on the right, and the middle one is centered.
3. Make items wrap to multiple lines instead of overflowing.

<details>
<summary>Answers</summary>

1. `display: flex; justify-content: center; align-items: center;` (parent needs a height for vertical).
2. `justify-content: space-between;`
3. `flex-wrap: wrap;`

</details>

## Mini challenge

Build a "toolbar" with 3 icons left-aligned and one help icon on the right. (Hint: combine space-between and `gap`.)

## What's next

Containers done. Now we customize the **items themselves** — flex-grow, flex-basis, alignment overrides.

→ [Lesson 39 — Flex Items in Depth](39-flexbox-items.md)
