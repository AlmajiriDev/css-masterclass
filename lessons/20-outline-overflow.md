# Lesson 20 — Outline, Overflow & Image Fitting

> **The pencil-tracing analogy.** A `border` is part of the box and changes layout. An `outline` is a pencil tracing around the box — it doesn't push other elements. Overflow is what happens when content overflows the box. And fitting images inside a box is one of the most common CSS puzzles.

## What you'll learn

- The `outline` property (often used for focus rings).
- `overflow: hidden | scroll | auto | visible`.
- `object-fit` and `object-position` for images and videos.
- When to use `overflow: hidden` carefully.

## Why this matters

Outlines help you see what's selected. Overflow helps you control bad content. And `object-fit: cover` is what makes product photos look good across card sizes.

## Outline

```css
.btn:focus-visible {
  outline: 3px solid #6d28d9;
  outline-offset: 4px;       /* gap between the element and the outline */
}
```

Outlines look like borders but:

- They **don't take up space** — no layout shift.
- They can be **non-rectangular** in some browsers (use `outline-style: solid` for rectangle).
- They are drawn on top of everything (z-order).

The most common use is the **focus ring** for keyboard navigation. **Never disable it** without a replacement.

```css
/* DO NOT do this for all users */
:focus { outline: none; }    /* kills accessibility */

/* Do this instead */
:focus { outline: none; }
:focus-visible { outline: 3px solid orange; }   /* only keyboard users */
```

## Overflow

```css
.box { overflow: hidden; }   /* cut off */
.box { overflow: scroll; }   /* always show scrollbar */
.box { overflow: auto; }     /* scroll only if needed */
.box { overflow: visible; }  /* default — let it spill out */
```

You can also set per-axis:

```css
.box {
  overflow-x: hidden;
  overflow-y: auto;
}
```

### Clever uses of `overflow: hidden`

- **Forms an "image mask"** — clip a square image into a circle.
- **Creates a new block formatting context**, killing margin collapse (Lesson 18).
- **Hides scrollbars in carousels** while allowing swiping.

### Auto-overflow gotcha

```css
.box {
  height: 100px;
  overflow: auto;
}
```

If content is shorter, no scrollbar. If content is taller, scrollbar appears. Use `auto` for everything dynamic.

## Image fitting — the most useful lesson here

`<img>` elements are weird. They have their own size, ignoring parent. CSS gives us `object-fit` to fix that.

```css
.card {
  width: 100%;
  aspect-ratio: 16 / 9;     /* lesson ahead */
}
.card img {
  width: 100%;
  height: 100%;
  object-fit: cover;        /* like background-size: cover */
  object-position: center;  /* like background-position */
}
```

### `object-fit` options

- `fill` — stretch to fill (default; usually ugly)
- `contain` — fit inside, leaving bars
- `cover` — fill, cropping edges
- `none` — keep original size
- `scale-down` — like contain but never upscale

### `object-position`

Like `background-position`:

```css
img { object-position: top; }   /* center is default; align to top */
```

## Try it yourself

Create `practice/lesson20.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 20</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body {
      font-family: system-ui, sans-serif;
      padding: 30px;
      background: #fffbf5;
    }

    .row { display: grid; gap: 16px; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); }

    .card {
      background: white;
      border-radius: 12px;
      box-shadow: 0 4px 14px rgba(0,0,0,0.08);
      overflow: hidden;
    }
    .card .frame {
      width: 100%;
      aspect-ratio: 16/9;
      background: #f5f3ff;
      overflow: hidden;
    }
    .card img, .card .svg {
      width: 100%;
      height: 100%;
      object-fit: cover;
      object-position: center;
      display: block;
    }
    .card .body {
      padding: 14px 16px;
    }

    /* square avatar gets cut to a circle */
    .avatar-frame {
      width: 96px; height: 96px;
      border-radius: 50%;
      overflow: hidden;
      background: #ede9fe;
    }
    .avatar-frame img {
      width: 100%; height: 100%;
      object-fit: cover;
    }

    /* scroll demo */
    .scrollbox {
      width: 320px;
      height: 100px;
      border: 1px solid #ddd;
      padding: 10px;
      overflow: auto;
      background: white;
    }

    /* focus ring */
    button {
      padding: 10px 16px;
      background: #6d28d9;
      color: white;
      border: 0;
      border-radius: 8px;
      cursor: pointer;
      margin: 6px;
    }
    button:focus-visible {
      outline: 3px solid #f97316;
      outline-offset: 3px;
    }
  </style>
</head>
<body>
  <h1>Outline, overflow, object-fit</h1>

  <h2>Tab to any button — see the outline</h2>
  <div>
    <button>One</button>
    <button>Two</button>
    <button>Three</button>
  </div>

  <h2>Cards with object-fit: cover (SVG images for portability)</h2>
  <div class="row">
    <div class="card">
      <div class="frame">
        <!-- SVG cat / peach / sky made with gradients -->
        <img alt="" src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 400 225'><rect width='100%' height='100%' fill='%236d28d9'/><circle cx='100' cy='110' r='45' fill='%23fde68a'/></svg>">
      </div>
      <div class="body"><b>Sunset</b><p>object-fit: cover keeps aspect ratio.</p></div>
    </div>
    <div class="card">
      <div class="frame">
        <img alt="" src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 400 225'><rect width='100%' height='100%' fill='%2310b981'/><rect x='40' y='40' width='320' height='145' fill='%23fde68a'/></svg>">
      </div>
      <div class="body"><b>Forest</b><p>Test with different aspect ratios.</p></div>
    </div>
    <div class="card">
      <div class="frame">
        <img alt="" src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 400 225'><rect width='100%' height='100%' fill='%230ea5e9'/><polygon points='0,225 400,225 200,80' fill='%23f1f5f9'/></svg>">
      </div>
      <div class="body"><b>Mountain</b><p>Always 16:9 thanks to aspect-ratio.</p></div>
    </div>
  </div>

  <h2>Square avatar → circle (overflow:hidden + border-radius)</h2>
  <div class="avatar-frame">
    <img alt="" src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 200 200'><rect width='100%' height='100%' fill='%23f97316'/><circle cx='100' cy='80' r='40' fill='%23fff'/></svg>">
  </div>

  <h2>Overflow: auto scrollbox</h2>
  <div class="scrollbox">
    <p>1</p><p>2</p><p>3</p><p>4</p><p>5</p><p>6</p><p>7</p>
  </div>
</body>
</html>
```

## Common mistakes

- **`outline: none` everywhere** — kills accessibility. Always replace with `:focus-visible`.
- **`overflow: hidden` "fixing" layout** — sure, but it might also hide content. Use with care.
- **Forgetting `object-fit: cover` on images** — they stretch.

## Debugging tips

- In DevTools, toggle `overflow` from `visible` to `hidden` to see what's spilling.
- In Computed → `object-fit` to see if your image rule applied.

## Quick quiz

1. Why is `outline` preferred over `border` for focus rings?
2. What does `overflow: auto` do that `scroll` doesn't?
3. What property crops an image to fit a fixed-aspect box?

<details>
<summary>Answers</summary>

1. Outline doesn't change layout (no shift when it appears), and is purely visual.
2. `scroll` shows the scrollbar always, even if not needed.
3. `object-fit: cover` (paired with width/height on the parent).

</details>

## Mini challenge

Make a 100×100 circular avatar in a card. Make the **whole card** clickable. Bonus: on focus-visible, show a 3px dashed outline.

## What's next

We can dress every element. Now we move them around — position.

→ [Lesson 21 — CSS Position & Offsets](21-position.md)
