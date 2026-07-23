# Lesson 19 — Display & Visibility

> **The traffic-flow analogy.** HTML elements are like cars on a road. Some are buses that take the whole lane and stop for everyone (`block`). Some are bicycles that squeeze into gaps and don't stop (`inline`). Some get parked in the garage (`none`). CSS `display` controls the lane.

## What you'll learn

- The 5 main `display` values you'll use: `block`, `inline`, `inline-block`, `none`, plus the flex/grid preview.
- Differences between `display: none` and `visibility: hidden`.
- When a `span` becomes a `div` and vice versa.

## Why this matters

Default behavior often surprises you. A `<div>` stacks vertically — perfect. A `<span>` flows inline — also perfect. The moment you need different behavior, `display` is the tool.

## The 5 main display values

### `block`

```css
div   { display: block; }     /* default */
section, h1, p, ul, li { display: block; }
```

Takes a full row. Always starts on a new line. **Accepts width, height, margin, padding** in full.

### `inline`

```css
span, a, em, strong { display: inline; }   /* default */
```

Flows inside a sentence. Takes only as much width as needed. Vertical margin/padding look weird. No explicit width/height.

### `inline-block`

```css
.tag {
  display: inline-block;
  background: #ede9fe;
  padding: 4px 10px;
  border-radius: 999px;
}
```

Best of both worlds: flows inline **and** accepts width/height/padding/margin.

### `none`

```css
.modal { display: none; }   /* completely removed from the page */
```

No space, no accessibility tree presence. As if it never existed.

### Flex, Grid, contents (preview)

```css
.row    { display: flex; }     /* Lesson 38 */
.grid   { display: grid; }     /* Lesson 40 */
.clean  { display: contents; }  /* box disappears, children remain */
```

`display: flex` and `display: grid` turn the element into a layout container for its children.

## `display: none` vs `visibility: hidden`

```css
.hidden     { display: none; }       /* gone */
.invisible  { visibility: hidden; }  /* still there, just not visible */
```

Differences:

| Property | Space taken | Animatable | Children |
|----------|-------------|------------|----------|
| `display: none` | gone (no space) | hard | also gone |
| `visibility: hidden` | yes (still takes space) | smooth (transitions) | can be overridden with `visibility: visible` |

`visibility: hidden` is useful for things like tooltips that should keep layout but be transparent.

## Common conversions

### Make a button from a link

```css
a.btn {
  display: inline-block;
  background: #6d28d9;
  color: white;
  padding: 12px 24px;
  border-radius: 8px;
  text-decoration: none;
}
```

### Make a `div` row of cards

```css
.row { display: flex; gap: 16px; flex-wrap: wrap; }
```

### A `div` that doesn't break the line

```css
.tag { display: inline-block; padding: 4px 10px; background: #ede9fe; border-radius: 999px; }
```

## Try it yourself

Create `practice/lesson19.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 19 — display</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body { font-family: system-ui, sans-serif; padding: 30px; background: #fffbf5; }

    /* block demo */
    .block-row > div {
      background: #ede9fe;
      padding: 10px;
      margin: 4px 0;
    }

    /* inline demo */
    .inline-row span {
      background: #fde68a;
      padding: 4px 6px;
    }
    /* vertical padding doesn't affect layout on inline */
    .inline-row span.span-tall {
      padding: 20px 6px;
    }

    /* inline-block demo */
    .pill { display: inline-block; padding: 6px 14px; margin: 4px 2px;
            background: #ddd6fe; border-radius: 999px; }

    .gone   { display: none; }
    .hidden { visibility: hidden; }

    .placeholder {
      background: #fbcfe8;
      padding: 10px;
      margin: 4px 0;
    }
  </style>
</head>
<body>
  <h1>Display</h1>

  <h2>block — new line, full width</h2>
  <div class="block-row">
    <div>First</div>
    <div>Second</div>
    <div>Third</div>
  </div>

  <h2>inline — flows in text</h2>
  <p class="inline-row">
    Some <span>inline</span> text with <span>yellow</span> spans.
    This one has <span class="span-tall">a lot of vertical padding but you can't really see it</span>.
  </p>

  <h2>inline-block — pills</h2>
  <div>
    <span class="pill">CSS</span><span class="pill">HTML</span><span class="pill">Flexbox</span>
    <span class="pill">Grid</span><span class="pill">Animations</span>
  </div>

  <h2>display: none vs visibility: hidden</h2>
  <div class="placeholder">A</div>
  <div class="placeholder gone">B (display:none — no space)</div>
  <div class="placeholder hidden">C (visibility:hidden — keeps space)</div>
  <div class="placeholder">D</div>

  <p style="color:#6b7280">A–D should leave gap missing B but present space for C.</p>
</body>
</html>
```

Open it. Watch the spaces.

## Common mistakes

- **`display: inline` then trying to give width/height** — they have no effect. Use `inline-block`.
- **`display: none` for animation** — animations don't run on elements with `display: none`. Use opacity + `pointer-events: none` to hide while animating.

## Debugging tips

- In DevTools → Computed, the `display` field shows the actual value used. A rule that says `block` but shows `inline` means a more specific rule won.

## Quick quiz

1. Name the 3 everyday display values and one difference each.
2. Which property hides an element while keeping its space?
3. A `<span>` won't get `width` set. What value of `display` fixes that?

<details>
<summary>Answers</summary>

1. `block` (full width, new line), `inline` (in-flow, no width/height), `inline-block` (in-flow but accepts width/height).
2. `visibility: hidden`.
3. `display: inline-block`.

</details>

## Mini challenge

Take a paragraph with mixed **inline** and **inline-block** "tags" and write a CSS rule using `:hover` to color them. Bonus: change the display of one of them and observe the difference.

## What's next

We shape and space elements. Now we look at the things around them: outlines, overflow, and how to fit tricky content like images.

→ [Lesson 20 — Outline, Overflow & Image Fitting](20-outline-overflow.md)
