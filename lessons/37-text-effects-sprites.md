# Lesson 37 — Image Sprites, Masks & Text Effects

> **The die-cut sticker analogy.** A sprite is many small images packed into one big image — the browser only loads one file instead of dozens, so your site is faster. A mask lets you make an element visible only through a shape. Text effects let you make text feel 3D, glowy, or worn.

## What you'll learn

- The image sprite technique.
- The `mask` property and how to use it.
- Cool text effects (truncate, multiline truncate, glow, gradient).

## Why this matters

Sprites are still used heavily in gaming-style UI. Masks are how logos and irregular shapes get clipped. Text effects close the visual polish loop.

## Image sprites

A "sprite" is one image with many icons. We move it around to show the one we want.

```css
.icon {
  width: 24px; height: 24px;
  background-image: url("sprite.png");
  background-size: 240px 24px;   /* full sprite is 10x bigger */
}
.icon-home   { background-position:    0 0; }
.icon-search { background-position:  -24px 0; }
.icon-user   { background-position:  -48px 0; }
```

Modern advice: skip this unless you must support really old browsers. SVG icons (Lesson 14) are easier and often smaller. But the technique still shows up in icon-font sprite files.

## Mask — cut out a shape

`mask` works like `clip-path` but uses **an image** as the shape.

```css
.logo {
  width: 200px;
  -webkit-mask: url("blob.svg") no-repeat center / contain;
  mask: url("blob.svg") no-repeat center / contain;
  background: linear-gradient(135deg, #6d28d9, #f97316);
}
```

The mask image's black areas hide the element; white areas show it; gray lets it pass through partially.

### A common pattern: gradient masked text

```html
<h1 class="masked">Masked</h1>
```

```css
.masked {
  font-size: 80px;
  font-weight: 900;
  background: linear-gradient(135deg, #6d28d9, #f97316);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
  text-align: center;
}
```

We saw this in Lesson 25. The mask is just the text shape.

## Text effects

### Truncate text in one line

```css
.truncate {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

### Truncate multiple lines

```css
.lines-3 {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
```

`-webkit-line-clamp` is supported everywhere in practice, even Firefox now.

### Glow

```css
.glow {
  color: white;
  text-shadow:
    0 0 6px rgba(255,255,255,.7),
    0 0 12px #f97316,
    0 0 18px #f97316;
}
```

### Gradient text

```css
.grad-text {
  background: linear-gradient(90deg, #6d28d9, #f97316);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
}
```

### Stroke / outline text

```css
.outline {
  -webkit-text-stroke: 2px #1e293b;
  color: transparent;
}
```

### Subtle shadow

```css
.subtle {
  text-shadow: 0 2px 4px rgba(0,0,0,0.2);
}
```

## Custom font styling tips

```css
small-caps-text {
  font-variant: small-caps;     /* Use small caps */
}

.numerals-table {
  font-variant-numeric: tabular-nums;  /* numbers align in tables */
}

.kerned {
  letter-spacing: 0.03em;
}
```

`font-variant-numeric: tabular-nums` keeps digits the same width — perfect for any data table.

## Try it yourself

Create `practice/lesson37.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 37</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body {
      font-family: system-ui, sans-serif;
      padding: 30px;
      background: #fffbf5;
      max-width: 800px;
      margin: 0 auto;
    }
    h2 { margin-top: 30px; }

    /* truncate */
    .truncate {
      width: 220px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      border: 1px solid #ddd;
      padding: 8px 12px;
      border-radius: 6px;
      background: white;
    }
    .lines-3 {
      display: -webkit-box;
      -webkit-line-clamp: 3;
      -webkit-box-orient: vertical;
      overflow: hidden;
      background: white;
      border-radius: 8px;
      padding: 12px 16px;
      border: 1px solid #ddd;
    }

    /* gradient text */
    .grad {
      font-size: 64px;
      font-weight: 900;
      letter-spacing: -2px;
      background: linear-gradient(90deg, #6d28d9, #f97316);
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
    }
    .glow {
      font-size: 48px;
      color: white;
      text-shadow:
        0 0 6px rgba(255,255,255,.7),
        0 0 14px #f97316,
        0 0 30px #f97316;
      background: #1e1b3a;
      padding: 30px;
      border-radius: 12px;
      text-align: center;
    }
    .outline-text {
      font-size: 60px;
      font-weight: 900;
      -webkit-text-stroke: 2px #1e293b;
      color: transparent;
      text-align: center;
    }

    /* mask demo using CSS gradient as the "shape" */
    .blob {
      width: 200px; height: 200px;
      background: linear-gradient(135deg, #6d28d9, #f97316);
      -webkit-mask: radial-gradient(circle, black 40%, transparent 60%);
      mask: radial-gradient(circle, black 40%, transparent 60%);
      border-radius: 50%;
    }

    /* tabular numbers in a fake receipt */
    .receipt {
      font-family: "SF Mono", monospace;
      background: white;
      padding: 16px;
      border-radius: 8px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.06);
      width: 320px;
    }
    .receipt .row {
      display: flex; justify-content: space-between;
      font-variant-numeric: tabular-nums;
    }
  </style>
</head>
<body>
  <h1>Sprites, masks, text effects</h1>

  <h2>Truncate one line</h2>
  <div class="truncate">A very very very long sentence that should be cut off with an ellipsis.</div>

  <h2>Truncate multiple lines (3)</h2>
  <div class="lines-3">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</div>

  <h2>Gradient text</h2>
  <div class="grad">BIG NEWS</div>

  <h2>Glow text</h2>
  <div class="glow">Feel the glow</div>

  <h2>Outline text</h2>
  <div class="outline-text">OUTLINE</div>

  <h2>Mask (radial)</h2>
  <div class="blob"></div>

  <h2>Receipt (tabular numerals)</h2>
  <div class="receipt">
    <div class="row"><span>Espresso</span><span>$3.50</span></div>
    <div class="row"><span>Croissant</span><span>$4.00</span></div>
    <div class="row"><span>Tax</span><span>$0.30</span></div>
    <div class="row" style="border-top:1px dashed #ddd; padding-top:6px; margin-top:6px"><b>Total</b><b>$7.80</b></div>
  </div>
</body>
</html>
```

## Common mistakes

- **`text-overflow: ellipsis` without `white-space: nowrap` and `overflow: hidden`** — won't show the ellipsis.
- **Forgetting `-webkit-` prefixes** for `background-clip` and `text-stroke` on Safari.
- **Trying to "mask with CSS gradient" without `mask:`** — you have to set `mask:`, not `background:`.

## Quick quiz

1. Write the three CSS lines needed to truncate text in one line.
2. How do you make a gradient text fill?
3. How do you make numbers align in a table visually?

<details>
<summary>Answers</summary>

1. `white-space: nowrap; overflow: hidden; text-overflow: ellipsis;`
2. Gradient `background-image` + `background-clip: text` + `color: transparent`.
3. `font-variant-numeric: tabular-nums;`

</details>

## Mini challenge

Build a "magazine headline" using two text effects: a giant gradient-filled `<h1>` and a smaller subtitle with a soft drop shadow.

## What's next

Now the big one — Flexbox. This is the muscle you'll use for layout more than anything else.

→ [Lesson 38 — Flexbox Fundamentals](38-flexbox.md)
