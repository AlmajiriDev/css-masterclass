# Lesson 40 — Grid Fundamentals

> **The chessboard analogy.** Flexbox puts things in a row or column. Grid puts things on a **2D chessboard** with rows and columns. You decide the size of each cell. Items can take one cell or many.

## What you'll learn

- How to turn an element into a grid.
- `grid-template-columns` and `grid-template-rows`.
- `gap` and `fr` units.
- `grid-auto-flow`.
- When to combine Grid with Flexbox.

## Why this matters

Grid was the leap that made magazine-quality layouts possible on the web. A 12-column responsive grid is the foundation of every modern design system.

## The basics

```html
<div class="grid">
  <div>1</div>
  <div>2</div>
  <div>3</div>
  <div>4</div>
</div>
```

```css
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 16px;
}
```

That's three equal columns with gaps. Done.

## `grid-template-columns`

```css
grid-template-columns: 200px 200px 200px;       /* three fixed columns */
grid-template-columns: 1fr 1fr 1fr;             /* three equal flexible columns */
grid-template-columns: repeat(3, 1fr);           /* same */
grid-template-columns: 200px 1fr 200px;          /* sidebar, content, sidebar */
grid-template-columns: 1fr 2fr 1fr;              /* 25 / 50 / 25 */
grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));   /* responsive grid */
```

The last one — `auto-fit` with `minmax` — is **the most useful line in CSS**:

```css
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 16px;
}
```

This makes as many columns as fit, each at least 220px. Resize the window — columns reflow automatically.

## `grid-template-rows`

Same syntax. Use when rows have a specific size or pattern:

```css
grid-template-rows: 100px 1fr 60px;       /* header, content, footer heights */
```

## `gap`

```css
gap: 16px;                  /* both row-gap and column-gap */
row-gap: 24px;
column-gap: 16px;
```

## `fr` units

`fr` is "free space fraction." It's relative:

```css
grid-template-columns: 1fr 2fr;          /* 1:2 ratio */
```

When combined with fixed sizes:

```css
grid-template-columns: 240px 1fr;        /* sidebar 240, rest is fluid */
```

## `grid-auto-flow`

```css
.grid { grid-auto-flow: row; }       /* default — fill row by row */
      { grid-auto-flow: column; }    /* fill column by column */
      { grid-auto-flow: dense; }     /* backfill holes (visual order breaks DOM order) */
```

## Practical: header / sidebar / content / footer

```css
.app {
  display: grid;
  grid-template-columns: 240px 1fr;
  grid-template-rows: 64px 1fr 56px;
  grid-template-areas:
    "sidebar header"
    "sidebar content"
    "sidebar footer";
  min-height: 100vh;
}
```

```html
<div class="app">
  <header class="header">H</header>
  <aside class="sidebar">S</aside>
  <main class="content">C</main>
  <footer class="footer">F</footer>
</div>
```

```css
.header  { grid-area: header; }
.sidebar { grid-area: sidebar; }
.content { grid-area: content; }
.footer  { grid-area: footer; }
```

`grid-template-areas` is a beautifully readable way to define layouts.

## Try it yourself

Create `practice/lesson40.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 40 — grid</title>
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
      box-shadow: 0 4px 12px rgba(0,0,0,0.06);
      margin-bottom: 16px;
    }

    .cell {
      background: #ede9fe;
      padding: 24px;
      border-radius: 8px;
      text-align: center;
      font-weight: 700;
      color: #1e293b;
    }
    .cell.two { background: #fde68a; }
    .cell.three { background: #d1fae5; }

    /* 3 equal columns */
    .equal { grid-template-columns: repeat(3, 1fr); }

    /* fixed + flex */
    .sidebar { grid-template-columns: 240px 1fr; }
    .sidebar .cell { padding: 14px; }

    /* 1:2:1 */
    .ratio { grid-template-columns: 1fr 2fr 1fr; }

    /* responsive grid (the magic line) */
    .responsive {
      grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    }

    /* page layout with grid-template-areas */
    .app {
      display: grid;
      grid-template-columns: 240px 1fr;
      grid-template-rows: 64px 1fr 56px;
      grid-template-areas:
        "side header"
        "side main"
        "side footer";
      min-height: 90vh;
      gap: 16px;
    }
    .app .header  { grid-area: header; background: #6d28d9; color: white; padding: 16px 20px; border-radius: 8px; display: flex; align-items: center; }
    .app .side    { grid-area: side;   background: #ede9fe; padding: 16px; border-radius: 8px; }
    .app .main    { grid-area: main;   background: white; padding: 20px; border-radius: 8px; border: 1px solid #eee; }
    .app .footer  { grid-area: footer; background: #fde68a; padding: 14px; border-radius: 8px; }
  </style>
</head>
<body>
  <h1>CSS Grid</h1>

  <h2>3 equal columns (1fr 1fr 1fr)</h2>
  <div class="grid equal">
    <div class="cell">1</div>
    <div class="cell two">2</div>
    <div class="cell three">3</div>
  </div>

  <h2>Sidebar + content (240px 1fr)</h2>
  <div class="grid sidebar">
    <div class="cell">Sidebar</div>
    <div class="cell two">Content</div>
  </div>

  <h2>1 : 2 : 1</h2>
  <div class="grid ratio">
    <div class="cell">1</div>
    <div class="cell two">2 (wide)</div>
    <div class="cell">1</div>
  </div>

  <h2>Responsive grid (resizing magic)</h2>
  <p style="color:#6b7280">Resize the window. Cards reflow.</p>
  <div class="grid responsive">
    <div class="cell">1</div><div class="cell">2</div><div class="cell">3</div>
    <div class="cell">4</div><div class="cell">5</div><div class="cell">6</div>
    <div class="cell">7</div><div class="cell">8</div>
  </div>

  <h2>grid-template-areas layout</h2>
  <div class="app">
    <div class="header">Header</div>
    <div class="side">Sidebar</div>
    <div class="main">Main content</div>
    <div class="footer">Footer</div>
  </div>
</body>
</html>
```

Resize the window — the responsive grid reflows.

## Common mistakes

- **Forgetting `display: grid`** — without it, `grid-template-columns` does nothing.
- **Confusing `grid-template-rows` with cells** — Grid auto-flows; you only set rows if you want specific heights.
- **Long text overflowing fixed columns** — wrap with `min-width: 0` or use `minmax(0, 1fr)`.

## Quick quiz

1. Write the columns rule for 4 equal columns with 16px gaps.
2. Make a grid where items can be as small as 200px or as big as 1fr, with as many per row as fit.
3. What's the grid-area for an item to span 2 cells in the same row?

<details>
<summary>Answers</summary>

1. `grid-template-columns: repeat(4, 1fr); gap: 16px;`
2. `grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));`
3. (Not in this lesson — covered next.)

</details>

## Mini challenge

Build a "magazine cover" layout: hero area on the top (full width), then a 1:2:1 grid below with three article cards.

## What's next

Now we make individual grid items span rows/columns and how to build the famous 12-column grid.

→ [Lesson 41 — Grid Items & 12-Column Layout](41-grid-items.md)
