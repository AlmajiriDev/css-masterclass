# Lesson 16 — Height, Width & Max-width

> **The clothes analogy.** A `width: 100%` is like a t-shirt that grows to fit exactly. A `max-width` is like saying "but no more than XL". A `min-width` is like "but at least medium". Together they make a t-shirt that's always a comfortable size.

## What you'll learn

- `width` and `height`.
- `min-width`, `max-width`, `min-height`, `max-height`.
- Why `max-width: 100%` is the "image saver" rule.
- Responsive units (`%`, `vw`, `clamp`) for sizing.

## Why this matters

Without min/max constraints, your elements become unmanageable on giant screens or tiny phones. This is where most of "responsive layout" lives.

## Width & height

```css
.box {
  width: 300px;
  height: 200px;
}
```

Set fixed sizes when the content **must** be a specific size (icons, avatars, fixed-aspect media). Otherwise avoid them — they stop content from breathing.

### What happens if content is too big?

By default, content **overflows** the box (text spills out, images poke through). To handle it:

```css
.box { overflow: hidden; }   /* cut off overflow */
.box { overflow: auto; }      /* show scrollbar if needed */
.box { overflow: scroll; }    /* always show scrollbar */
```

We'll cover overflow in Lesson 20.

## Min- and max-width

These are softer than the `width` property.

```css
.container {
  width: 90%;
  max-width: 800px;
  margin: 0 auto;
}
```

This means:

- On phones (≤ 800px wide), the container is 90% of the screen.
- On desktop (1000+ px wide), it caps at 800px and centers itself.

That's the **most-used layout pattern** in CSS. Memorize it.

### `min-width`

The opposite: the box can't shrink below a value.

```css
.gallery { min-width: 240px; }   /* never smaller */
```

Useful with `display: flex` to stop items from collapsing.

### `min-height`

```css
.hero { min-height: 100vh; }
```

A fullscreen section that grows if content needs more.

## The "image saver"

```css
img {
  max-width: 100%;
  height: auto;
}
```

Images scale down to fit their parent but never overflow. **`height: auto` keeps the aspect ratio.** This pair solves 90% of image-layout problems.

## Width with viewport units

```css
.sidebar {
  width: 80vw;
  max-width: 320px;
}
```

80% of viewport width, but never wider than 320px. Great for off-canvas menus.

## Try it yourself

Create `practice/lesson16.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 16 — width/max-width</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body {
      font-family: system-ui, sans-serif;
      background: #fffbf5;
      margin: 0;
      padding: 0 20px 30px;
    }
    h1 { padding: 20px 0; }

    /* the classic responsive container */
    .container {
      width: 90%;
      max-width: 800px;
      margin: 0 auto 24px;
      padding: 24px;
      background: #ede9fe;
      border-radius: 12px;
    }

    /* full-bleed banner, capped */
    .hero {
      width: 100%;
      max-width: 1200px;
      margin: 0 auto;
      min-height: 60vh;
      padding: 30px;
      background: linear-gradient(135deg, #6d28d9, #f97316);
      color: white;
      border-radius: 12px;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    /* fixed-height with scroll */
    .scrollbox {
      width: 100%;
      max-width: 600px;
      margin: 0 auto;
      height: 200px;
      overflow: auto;
      background: white;
      border: 1px solid #ddd;
      padding: 16px;
      border-radius: 8px;
    }

    /* img saver demo */
    .img-saver {
      width: 90%;
      max-width: 400px;
      margin: 24px auto;
      display: block;
    }

    .responsive {
      width: 100%;
      max-width: 400px;
      height: auto;
    }
  </style>
</head>
<body>
  <h1>Width, max-width, friends</h1>

  <div class="container">
    <h2>Classic responsive container</h2>
    <p>width: 90%, max-width: 800px. Resize the window — it adapts but never wider than 800px.</p>
  </div>

  <div class="hero">
    <h2 style="margin:0">A hero that grows with content</h2>
  </div>

  <h2 style="text-align:center">Scroll box (200px tall, scrolls if needed)</h2>
  <div class="scrollbox">
    <p>1</p><p>2</p><p>3</p><p>4</p><p>5</p><p>6</p><p>7</p><p>8</p>
    <p>9</p><p>10</p><p>11</p><p>12</p>
  </div>

  <div class="img-saver">
    <p>Resize me — the parent is 90% × max 400px.</p>
    <img class="responsive" alt="colorful gradient"
         src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 400 200'><defs><linearGradient id='g' x1='0' y1='0' x2='1' y2='1'><stop offset='0' stop-color='%236d28d9'/><stop offset='1' stop-color='%23f97316'/></linearGradient></defs><rect width='100%' height='100%' fill='url(%23g)'/></svg>">
  </div>
</body>
</html>
```

Resize the window back and forth. The container breathes. The hero grows. The image stays sharp.

## Common mistakes

- **Fixed `width` on a parent** — kills responsiveness. Use `max-width` + `%`/`vw`.
- **Forgetting `height: auto` on `<img>`** — stretches them ugly.
- **`min-width: 0` needed inside Flexbox** to shrink children fully. We'll hit this in Lesson 39.

## Debugging tips

- To spot what's making your layout wide, in DevTools select any overflowing element and check its width. The element with `width` greater than the viewport is the culprit.

## Quick quiz

1. What's the difference between `width: 500px` and `max-width: 500px`?
2. Write the two-line rule that makes an image scale down without breaking aspect.
3. What's `min-height: 100vh` good for?

<details>
<summary>Answers</summary>

1. `width: 500px` forces exactly 500px. `max-width: 500px` allows narrower, never wider.
2. `img { max-width: 100%; height: auto; }`
3. Sections that fill the screen at minimum, even if content is short (hero sections).

</details>

## Mini challenge

Build a sidebar that's 80vw wide **and** never wider than 320px, and a main area that takes the rest. Hint: use Flexbox from Lesson 38 OR just inspect how the page behaves — your call.

## What's next

Boxes have measurements. Now we put **bubble wrap inside** — padding.

→ [Lesson 17 — Padding](17-padding.md)
