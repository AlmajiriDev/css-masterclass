# Lesson 41 — Grid Items & 12-Column Layout

> **The Tetris analogy.** In Lesson 40 we set up the grid. Now we choose each piece's **shape** — how many columns and rows it spans. We also build the famous 12-column responsive grid that powers Bootstrap, Tailwind, and most design systems.

## What you'll learn

- Spanning rows and columns.
- `grid-column`, `grid-row`.
- `justify-self` and `align-self` on items.
- The 12-column responsive layout pattern.
- Auto-fill vs auto-fit.

## Why this matters

Spanning lets one card take 6 of 12 columns while another takes 3. Build a hero "8 of 12 + sidebar 4 of 12" instantly. 12-column grids are the lingua franca of responsive web design.

## Spanning cells

```html
<div class="grid">
  <div class="a">Wide</div>
  <div class="b">Square</div>
  <div class="c">Tall</div>
  <div class="d">Square</div>
</div>
```

```css
.grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: 120px 120px;
  gap: 16px;
}

.a { grid-column: span 3; }     /* spans 3 of 4 columns */
.b { grid-column: span 1; }     /* 1 column */
.c { grid-row: span 2; }        /* spans 2 rows */
.d { grid-column: span 1; }
```

The grid auto-places them, but you tell specific items to span more.

## The precise way — line numbers

```css
.a {
  grid-column-start: 1;
  grid-column-end:   4;       /* spans columns 1, 2, 3 (4 is the next line) */
}
/* shorthand */
.a { grid-column: 1 / 4; }     /* same */
.a { grid-column: 1 / span 3; }   /* start at column 1, span 3 */
```

Same for `grid-row`.

## `justify-self` and `align-self`

Override the parent's `justify-items` (default `stretch`) per item:

```css
.a { justify-self: end; }      /* align right horizontally */
.a { align-self:   start; }     /* align top vertically */
```

## The 12-column responsive grid

The classic pattern, made of 12 narrow columns:

```css
.container {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: 16px;
}

.col-6  { grid-column: span 6; }
.col-4  { grid-column: span 4; }
.col-3  { grid-column: span 3; }
.col-12 { grid-column: span 12; }
```

Now:

```html
<section class="container">
  <article class="col-8">Main content</article>
  <aside class="col-4">Sidebar</aside>
</section>
```

8 + 4 = 12. The other 4 columns are empty and the sidebar sits on the right.

### Responsive 12-column with media queries

```css
.col-md-12 { grid-column: span 12; }
.col-md-6  { grid-column: span 12; }
.col-md-4  { grid-column: span 12; }

@media (min-width: 700px) {
  .col-md-6  { grid-column: span 6; }
  .col-md-4  { grid-column: span 4; }
  .col-md-12 { grid-column: span 12; }
}
```

You'll see this in Bootstrap's class names (`.col-md-6`).

### The auto-fit `minmax` pattern

```css
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(280px, 100%), 1fr));
  gap: 16px;
}
```

`auto-fit` collapses empty tracks (when content is fewer), `auto-fill` keeps them.

```css
grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));  /* keeps empty slots */
grid-template-columns: repeat(auto-fit,  minmax(200px, 1fr));  /* collapses empty slots */
```

## Spanning rows for tall cells

```css
.feature {
  grid-row: span 2;     /* takes 2 rows */
}
```

## Try it yourself

Create `practice/lesson41.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 41 — grid items</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body { font-family: system-ui, sans-serif; padding: 30px; background: #fffbf5; }
    h2 { color: #1e293b; margin-top: 30px; }

    .grid {
      display: grid;
      gap: 16px;
      padding: 16px;
      background: white;
      border-radius: 12px;
      box-shadow: 0 4px 14px rgba(0,0,0,0.06);
      margin-bottom: 20px;
    }
    .cell {
      background: #ede9fe;
      padding: 30px;
      border-radius: 8px;
      text-align: center;
      font-weight: 700;
      color: #1e293b;
      min-height: 80px;
    }
    .a { background: #6d28d9; color: white; }
    .b { background: #f97316; color: white; }
    .c { background: #fde68a; }
    .d { background: #d1fae5; }

    /* 1. spans */
    .s1 { grid-template-columns: repeat(4, 1fr); grid-template-rows: 100px 100px; }
    .s1 .a { grid-column: span 3; }
    .s1 .c { grid-row: span 2; }

    /* 2. self-alignment */
    .s2 { grid-template-columns: repeat(3, 1fr); grid-template-rows: 120px; }
    .s2 .a { justify-self: end;   align-self: start; }
    .s2 .b { justify-self: center; align-self: center; }
    .s2 .c { justify-self: start; align-self: end; }

    /* 3. 12-column system */
    .twelve { grid-template-columns: repeat(12, 1fr); }
    .twelve .col-8 { grid-column: span 8; }
    .twelve .col-4 { grid-column: span 4; }
    .twelve .hero  { grid-column: span 12; }
    .twelve .col-3 { grid-column: span 3; }

    /* 4. auto-fit vs auto-fill (small browser-resize demo) */
    .af { grid-template-columns: repeat(auto-fit, minmax(160px, 1fr)); }
  </style>
</head>
<body>
  <h1>Grid items &amp; 12-column</h1>

  <h2>1. Spanning</h2>
  <div class="grid s1">
    <div class="cell a">A (3 cols)</div>
    <div class="cell b">B</div>
    <div class="cell c">C (2 rows)</div>
    <div class="cell d">D</div>
  </div>

  <h2>2. justify-self &amp; align-self</h2>
  <div class="grid s2">
    <div class="cell a">end / start</div>
    <div class="cell b">center / center</div>
    <div class="cell c">start / end</div>
  </div>

  <h2>3. 12-column page</h2>
  <div class="grid twelve">
    <div class="cell hero" style="padding: 60px">Hero (col-12)</div>
    <div class="cell a" style="padding: 60px; grid-column: span 8">Article (col-8)</div>
    <div class="cell b" style="padding: 60px; grid-column: span 4">Sidebar (col-4)</div>
    <div class="cell c" style="padding: 40px">Card A (col-3)</div>
    <div class="cell" style="padding: 40px; background:#d1fae5">Card B (col-3)</div>
    <div class="cell b" style="padding: 40px; grid-column: span 6">Wide feature (col-6)</div>
  </div>

  <h2>4. auto-fit (resize the window)</h2>
  <p style="color:#6b7280">Each card at least 160px wide, as many fit per row as possible.</p>
  <div class="grid af">
    <div class="cell">A</div>
    <div class="cell a">B</div>
    <div class="cell b">C</div>
    <div class="cell c">D</div>
    <div class="cell">E</div>
    <div class="cell a">F</div>
    <div class="cell">G</div>
  </div>
</body>
</html>
```

## Common mistakes

- **`grid-column: span 13;`** on a 12-column grid — spills over; the item disappears if there's no track 13.
- **Confusing column-line numbers** — line 1 is the START (left edge), line 13 is the AFTER-LAST (right edge) for a 12-col grid. To span columns 1, 2, 3, write `1 / 4`.
- **Using `auto-fill` when you want items to grow** — use `auto-fit` instead.

## Quick quiz

1. Make a grid item take the **first 6 columns of a 12-column layout**.
2. Span 2 rows starting at row 2.
3. What's the difference between `auto-fill` and `auto-fit`?

<details>
<summary>Answers</summary>

1. `grid-column: span 6;`
2. `grid-row: 2 / span 2;`
3. `auto-fill` keeps empty tracks (slots). `auto-fit` collapses them so items can grow.

</details>

## Mini challenge

Build a "magazine cover" using the 12-column system: a 12-column hero on top, then a row of 8+4 (article + sidebar), then 3 equal cards (each col-4). Use only `<div class="grid">`.

## What's next

We have Grid. Now some advanced tools — multi-column text, masking, the `@property` rule.

→ [Lesson 42 — Multiple Columns & Masking](42-columns-masking.md)
