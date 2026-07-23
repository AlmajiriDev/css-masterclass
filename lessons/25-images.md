# Lesson 25 — Image Centering, Filters & Shapes

> **The Instagram analogy.** We've all squinted at a photo and said "this needs more saturation" or "this needs to be round." CSS now gives us image filters, transforms, and shape clipping — your photos can get the same treatment on a website without Photoshop.

## What you'll learn

- Image filters: blur, grayscale, brightness, contrast, hue rotate, etc.
- Image shapes: `clip-path`.
- Image hover effects.
- Using SVG `mask` for transparent shapes.

## Why this matters

A well-filtered image makes a page feel polished. A circle avatar reads instantly as a person. These small touches are what make a UI feel like 2024 not 2008.

## Image filters

```css
img {
  filter: grayscale(100%);          /* black & white */
  filter: blur(4px);                /* out of focus */
  filter: brightness(0.6);          /* darker */
  filter: contrast(1.5);            /* more punch */
  filter: hue-rotate(90deg);        /* shift colors */
  filter: invert(1);                /* inverted colors */
  filter: saturate(2);              /* extra vivid */
  filter: sepia(0.6);               /* old photo */
}
```

### Multiple filters

```css
img {
  filter: brightness(0.8) contrast(1.2) saturate(1.4);
}
```

### Hover effects

```css
.card img {
  transition: filter .3s;
}
.card:hover img {
  filter: brightness(0.7) contrast(1.3);
}
```

## `backdrop-filter` — fancy effects behind something

```css
.modal-overlay {
  background: rgba(255,255,255,0.2);
  backdrop-filter: blur(10px);
}
```

This blurs whatever is **behind** the element. Apple uses it everywhere.

## `clip-path` — shape the element

```css
.blob {
  width: 200px;
  height: 200px;
  background: #6d28d9;
  clip-path: circle(50%);                  /* full circle */
  clip-path: polygon(50% 0, 100% 50%, 50% 100%, 0 50%);   /* diamond */
  clip-path: path("M 0 0 L 200 0 L 200 200 L 0 200 Z");  /* custom path */
  clip-path: ellipse(40% 50% at 50% 50%); /* flat egg */
}
```

Useful functions:

- `circle(radius)` — perfect circles.
- `ellipse(rx ry at x y)` — stretched circles.
- `polygon(x y, x y, ...)` — straight-edged shapes.
- `path("...")` — any SVG path (powerful).

### Animating `clip-path`

```css
.menu {
  clip-path: circle(0% at top right);
  transition: clip-path .3s;
}
.menu.open {
  clip-path: circle(150% at top right);
}
```

Beautiful for menu transitions.

## `mask` — use an image as a mask

```css
.text-outline {
  background: url("photo.jpg");
  background-size: cover;
  color: transparent;
  -webkit-background-clip: text;
  background-clip: text;
}
```

This produces **text filled with an image**. Great for hero headlines.

## Mask vs clip-path

- `clip-path` — cuts the element based on a shape.
- `mask` — uses an image's alpha (transparency) as the shape. More flexible but heavier.

## Try it yourself

Create `practice/lesson25.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 25 — images</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body {
      font-family: system-ui, sans-serif;
      padding: 30px;
      background: #fffbf5;
    }
    .grid { display: grid; gap: 16px; grid-template-columns: repeat(auto-fit, minmax(160px, 1fr)); }

    .pic {
      width: 100%;
      aspect-ratio: 1/1;
      border-radius: 12px;
      overflow: hidden;
      background: linear-gradient(135deg, #6d28d9, #f97316);
      display: flex;
      align-items: flex-end;
      justify-content: center;
      color: white;
      font-weight: 700;
      text-shadow: 0 2px 4px rgba(0,0,0,0.4);
    }

    /* Filters */
    .f-grayscale { filter: grayscale(100%); }
    .f-blur      { filter: blur(4px); }
    .f-sepia     { filter: sepia(0.7); }
    .f-hue       { filter: hue-rotate(90deg); }
    .f-contrast  { filter: contrast(1.5) saturate(1.3); }

    /* Clip-paths */
    .c-circle {
      clip-path: circle(50%);
    }
    .c-blob {
      clip-path: polygon(50% 0%, 80% 20%, 100% 50%, 80% 80%, 50% 100%, 20% 80%, 0% 50%, 20% 20%);
    }
    .c-arrow {
      clip-path: polygon(0 0, 100% 0, 100% 75%, 60% 75%, 50% 100%, 40% 75%, 0 75%);
    }
    .c-hex {
      clip-path: polygon(25% 7%, 75% 7%, 100% 50%, 75% 93%, 25% 93%, 0 50%);
    }

    /* Image-filled text */
    .image-text {
      font-size: 80px;
      font-weight: 900;
      background: linear-gradient(135deg, #6d28d9 50%, #f97316 50%);
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
      text-align: center;
      letter-spacing: -2px;
      margin: 40px 0;
    }

    /* Backdrop filter demo */
    .backdrop-demo {
      position: relative;
      height: 200px;
      background:
        linear-gradient(135deg, #f97316 0%, #6d28d9 100%),
        repeating-linear-gradient(45deg, white 0 20px, transparent 20px 40px);
      background-blend-mode: overlay;
      border-radius: 12px;
      padding: 30px;
      color: white;
    }
    .frost {
      width: 220px;
      padding: 14px 18px;
      margin: 30px auto;
      border-radius: 12px;
      background: rgba(255,255,255,0.25);
      backdrop-filter: blur(12px);
      border: 1px solid rgba(255,255,255,0.4);
    }
  </style>
</head>
<body>
  <h1>Image filters, clip-paths, masks</h1>

  <h2>Filters</h2>
  <div class="grid">
    <div class="pic">original</div>
    <div class="pic f-grayscale">grayscale</div>
    <div class="pic f-blur">blur(4px)</div>
    <div class="pic f-sepia">sepia</div>
    <div class="pic f-hue">hue 90°</div>
    <div class="pic f-contrast">contrast + saturation</div>
  </div>

  <h2>Clip-paths (shapes)</h2>
  <div class="grid">
    <div class="pic c-circle">circle</div>
    <div class="pic c-blob">blob</div>
    <div class="pic c-arrow">tag arrow</div>
    <div class="pic c-hex">hexagon</div>
  </div>

  <h2>Image-text mask</h2>
  <h1 class="image-text">CSS IS MAGIC</h1>

  <h2>backdrop-filter</h2>
  <div class="backdrop-demo">
    <div class="frost">This panel blurs what's behind it.</div>
  </div>
</body>
</html>
```

## Common mistakes

- **Filters permanently destroyed** — `filter: blur(10px)` is permanent. To undo, animate back.
- **`clip-path` removes visible content** — children can also be clipped (and sometimes hidden).
- **`-webkit-background-clip: text`** — Safari/Chrome still need the prefixed version. Add both.

## Debugging tips

- Use [bennettfeely.com/clippy](https://bennettfeely.com/clippy/) to draw `clip-path` shapes visually.
- View source on a site you love and copy its filter combos.

## Quick quiz

1. Write a filter rule that turns an image black and white with slight contrast.
2. What's the difference between `clip-path` and `mask`?
3. Which property blurs the **content behind** an element?

<details>
<summary>Answers</summary>

1. `filter: grayscale(100%) contrast(1.1);`
2. `clip-path` uses a shape to cut. `mask` uses an image's transparency.
3. `backdrop-filter`.

</details>

## Mini challenge

Make a portfolio grid of 4 square tiles, each a different shape (`circle`, `blob`, `hexagon`, `tag arrow`) using only `clip-path` and a gradient background.

## What's next

We can dress up anything. Now we change CSS based on **state** — pseudo-classes.

→ [Lesson 26 — Pseudo-classes](26-pseudo-classes.md)
