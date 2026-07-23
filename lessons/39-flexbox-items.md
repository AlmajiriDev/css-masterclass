# Lesson 39 — Flex Items in Depth

> **The piece-of-cake analogy.** We learned to set the table (flex containers) in Lesson 38. Now we decide **how big each piece** is, **whether it can grow**, **whether it can shrink**, and **where it sits** when the others around it behave unexpectedly.

## What you'll learn

- `flex-grow`, `flex-shrink`, `flex-basis`.
- The `flex` shorthand.
- `align-self` (overrides align-items for one item).
- `order` (visual reorder).
- Common patterns: sidebars, equal columns, content-first footer.

## Why this matters

Containers tell items how to share space. Items tell the container "give me more" or "let me stay small." Together you get bulletproof layouts.

## The 3 properties on the items

```css
.item {
  flex-grow:    0;     /* can I take extra free space? (0 = no) */
  flex-shrink:  1;     /* can I shrink if needed? (1 = yes) */
  flex-basis:   auto;  /* my starting size before grow/shrink kicks in */
}
```

Defaults: `0 1 auto`. That is: don't grow, do shrink if needed, start at my content's natural size.

### The mental model

Imagine a row of 3 items, fixed width. After laying them out, there might be **extra space** or **not enough**. The container hands out "grow tokens" to items with `flex-grow > 0` and "shrink tokens" to items with `flex-shrink > 0`.

### Examples

```css
.col        { flex: 1; }                 /* equal share of remaining space */
.col-wide   { flex: 2; }                 /* twice as wide as `flex: 1` */
.fixed      { flex: 0 0 240px; }         /* fixed 240px, no grow, no shrink */
.grow-only  { flex: 1 0 0; }             /* equal share, no shrink */
.shrink-only { flex: 0 1 0; }            /* content only, shrink if needed */
```

## The `flex` shorthand

```css
flex: <grow> <shrink> <basis>;
flex: 1;          /* 1 1 0% — equal share */
flex: auto;       /* 1 1 auto — content-sized, shares remaining */
flex: none;       /* 0 0 auto — rigid size */
flex: 0 1 200px;  /* shrinkable, fixed starting size */
```

The most common patterns are `flex: 1` (equal columns) and `flex: auto` (content-sized).

## `align-self`

A single item can override the parent's `align-items`:

```css
.row  { display: flex; align-items: stretch; }
.row  { min-height: 200px; }
.row .tall { align-self: flex-start; }
.row .short { align-self: flex-end; }
```

## `order`

```css
.row a:nth-child(1) { order: 3; }
.row a:nth-child(2) { order: 1; }
.row a:nth-child(3) { order: 2; }
```

Reorders items **visually** without changing HTML. Default `order: 0`. Negative numbers allowed. **Use sparingly** — order shifts confuse screen reader users unless also done in the HTML.

## Common patterns

### Pattern: Sidebar + content

```css
.app {
  display: flex;
  min-height: 100vh;
}
.sidebar {
  flex: 0 0 240px;       /* fixed sidebar */
}
.content {
  flex: 1;               /* take all remaining width */
}
```

### Pattern: Equal columns

```css
.row { display: flex; gap: 16px; }
.col { flex: 1; }       /* all equal */
```

### Pattern: Content-first with footer

```css
.page {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}
.page main { flex: 1; }   /* takes remaining space, pushes footer to bottom */
```

### Pattern: "Push right"

```css
.row { display: flex; gap: 12px; }
.row .push { margin-left: auto; }   /* everything after this is pushed to the right */
```

This is how the "logo on left, nav on right" trick works.

## Gotcha — min-width

By default, flex items have `min-width: auto` — they don't shrink below their content size. For text items, this can cause horizontal overflow.

```css
.item {
  min-width: 0;          /* allow shrinking below content size */
}
```

Common in nav bars with long words.

## Try it yourself

Create `practice/lesson39.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 39 — flex items</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body { font-family: system-ui, sans-serif; padding: 30px; background: #fffbf5; }
    h2 { margin-top: 30px; color: #1e293b; }

    .row {
      display: flex;
      gap: 12px;
      padding: 16px;
      background: white;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.06);
      margin-bottom: 16px;
    }
    .item {
      background: #ede9fe;
      padding: 16px;
      border-radius: 8px;
      text-align: center;
      color: #1e293b;
      font-weight: 700;
    }
    .item.two { background: #fde68a; }
    .item.three { background: #d1fae5; }

    /* 1. flex: 0 0 X — fixed */
    .fixed .item { flex: 0 0 100px; }
    /* 2. flex: 1 — equal */
    .equal .item { flex: 1; }
    /* 3. flex: 2 1 0 — wide center */
    .widecenter .item:first-child,
    .widecenter .item:last-child { flex: 1; }
    .widecenter .item.two { flex: 2; }

    /* 4. sidebar pattern */
    .layout {
      display: flex;
      min-height: 200px;
    }
    .sidebar {
      flex: 0 0 200px;
      background: #6d28d9;
      color: white;
      padding: 20px;
      border-radius: 12px 0 0 12px;
    }
    .main {
      flex: 1;
      background: #ede9fe;
      padding: 20px;
      border-radius: 0 12px 12px 0;
    }

    /* 5. push right */
    .navbar { background: white; border-radius: 12px; padding: 14px 20px; box-shadow: 0 4px 12px rgba(0,0,0,0.06); display: flex; gap: 8px; align-items: center; margin-bottom: 16px; }
    .navbar a { padding: 6px 12px; border-radius: 6px; text-decoration: none; color: #1e293b; }
    .navbar a:hover { background: #ede9fe; }
    .navbar .push { margin-left: auto; }

    /* 6. align-self */
    .cross {
      display: flex; min-height: 120px;
      align-items: flex-start;
    }
    .cross .item.big   { padding: 40px 16px; }
    .cross .item.dif   { align-self: flex-end; }
    .cross .item.middle { align-self: center; }

    /* 7. min-width: 0 fix */
    .noscroll .item { min-width: 0; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  </style>
</head>
<body>
  <h1>Flexbox items</h1>

  <h2>Fixed (flex: 0 0 100px)</h2>
  <div class="row fixed">
    <div class="item">1</div>
    <div class="item">2</div>
    <div class="item">3</div>
    <div class="item">4</div>
  </div>

  <h2>Equal (flex: 1)</h2>
  <div class="row equal">
    <div class="item">1</div>
    <div class="item two">2</div>
    <div class="item three">3</div>
  </div>

  <h2>Wide center (flex: 2)</h2>
  <div class="row widecenter">
    <div class="item">1</div>
    <div class="item two">2 (twice as wide)</div>
    <div class="item">3</div>
  </div>

  <h2>Sidebar + content layout</h2>
  <div class="layout">
    <div class="sidebar">Sidebar<br>(200px fixed)</div>
    <div class="main">Main content (rest)</div>
  </div>

  <h2>Navbar with push-right</h2>
  <nav class="navbar">
    <a>Home</a><a>About</a><a>Blog</a>
    <a class="push">Contact</a>
  </nav>

  <h2>align-self overrides</h2>
  <div class="row cross">
    <div class="item">top</div>
    <div class="item middle">middle</div>
    <div class="item">top</div>
    <div class="item dif">bottom</div>
  </div>
</body>
</html>
```

## Common mistakes

- **`flex: 1` on items that are also `display: flex`** — the parent is fine, but the child's growth isn't always what you expect. Test.
- **Long content forcing layout break** — apply `min-width: 0` to the affected item.
- **Reordering with `order`** — breaks the tab order for keyboard users. Best to keep DOM order.

## Quick quiz

1. Make 3 equal columns.
2. Make a 240px sidebar with the rest taking the remaining space.
3. What's the difference between `flex: 1` and `flex: auto`?

<details>
<summary>Answers</summary>

1. `flex: 1` on each.
2. Sidebar: `flex: 0 0 240px`. Main: `flex: 1`.
3. `flex: 1` is `1 1 0%` (equal share, can shrink). `flex: auto` is `1 1 auto` (start at content size, then share growth).

</details>

## Mini challenge

Build a "chat" layout where the sidebar holds a list of conversations (250px fixed), and the right side flexes. Inside the right side, the message area grows to fill the height and the input stays at the bottom.

## What's next

Now the other 2D layout weapon: **CSS Grid**.

→ [Lesson 40 — Grid Fundamentals](40-grid.md)
