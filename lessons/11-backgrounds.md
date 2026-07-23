# Lesson 11 — Backgrounds

> **The wallpaper analogy.** A background is the wallpaper behind the words. It can be flat paint, a pattern, a photo, or a gradient. CSS lets you tile it, position it, and resize it without touching the HTML.

## What you'll learn

- Background color, image, repeat, position, size, attachment.
- The `background` shorthand.
- Background blends and overlays.
- Multiple backgrounds stacked in one element.

## Why this matters

Backgrounds turn a flat page into a mood. Used well, they add depth. Used poorly, they look like 2003 MySpace.

## The 8 `background-*` properties

```css
.hero {
  background-color: #fef3c7;                 /* color, fallback */
  background-image: url("hero.jpg");          /* image */
  background-repeat: no-repeat;               /* don't tile */
  background-size: cover;                     /* fill area, crop edges */
  background-position: center;                /* center */
  background-attachment: fixed;               /* stay put on scroll */
  background-origin: padding-box;             /* anchor point */
  background-clip: padding-box;               /* paint area */
}
```

Let's go one by one.

### `background-color`

```css
.card { background-color: #ede9fe; }
```

A solid color. Always set this **in addition to** a background image — the image might fail and the color shows through.

### `background-image`

```css
.bg { background-image: url("stars.png"); }
```

Accepts:
- `url(...)` for files (relative or absolute path).
- `linear-gradient(...)` and `radial-gradient(...)`.
- Multiple images separated by commas.

### `background-repeat`

```css
.tile { background-repeat: repeat; }       /* default — tile both ways */
.x    { background-repeat: repeat-x; }     /* tile only horizontally */
.no   { background-repeat: no-repeat; }    /* never tile */
```

You can also use two values: `repeat-x space` (tile horizontally with equal space).

### `background-size`

```css
.cover   { background-size: cover; }       /* fill area, may crop */
.contain { background-size: contain; }     /* fit inside, may leave gaps */
.fixed   { background-size: 300px 200px; } /* exact size */
```

`cover` is your friend for hero photos. `contain` is your friend for logos/patterns.

### `background-position`

```css
.center { background-position: center; }
.left   { background-position: left center; }
.pos    { background-position: 20% 80%; }   /* x y */
```

Pairs with `no-repeat` or with `cover`.

### `background-attachment`

```css
.fixed  { background-attachment: fixed; }   /* parallax-like */
.scroll { background-attachment: scroll; }   /* default — scrolls with content */
.local  { background-attachment: local; }   /* scrolls with the content inside */
```

Use `fixed` sparingly — it can cause performance issues on mobile.

## Multiple backgrounds

Stack many backgrounds in a single rule:

```css
.banner {
  background-image:
    linear-gradient(rgba(0,0,0,0.5), rgba(0,0,0,0.7)),
    url("mountains.jpg");
  background-size: cover;
  background-position: center;
  color: white;
}
```

The first one in the list is on **top**. Each layer can have its own `background-position` / `background-size`:

```css
.multi {
  background-image: url("logo.png"), url("pattern.png");
  background-size: 80px, 200px;
  background-repeat: no-repeat, repeat;
  background-position: top right, top left;
}
```

## The shorthand

```css
.hero {
  background: #fef3c7 url("hero.jpg") center / cover no-repeat fixed;
}
```

Order:

```
[color] [image] [position] / [size] [repeat] [attachment] [origin] [clip]
```

Any piece you skip is reset to the default. The `position / size` pair must be separated by `/`.

### Extra: gradients

```css
.bg {
  background: linear-gradient(135deg, #6d28d9, #f97316);
}
```

We cover gradients deeply in Lesson 13.

## Try it yourself

Create `practice/lesson11.html`. (We'll use simple SVG/CSS patterns so we have no external files.)

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 11 — backgrounds</title>
  <style>
    body {
      font-family: system-ui, sans-serif;
      padding: 0;
      margin: 0;
    }
    section {
      padding: 40px 24px;
      color: white;
      min-height: 200px;
      text-shadow: 0 2px 6px rgba(0,0,0,0.6);
    }

    /* 1. solid color */
    .solid   { background-color: #6d28d9; }

    /* 2. CSS-only pattern via gradient */
    .pattern {
      background-image:
        radial-gradient(circle, #ffffff 1.5px, transparent 2px),
        linear-gradient(135deg, #f97316, #b45309);
      background-size: 24px 24px, 100% 100%;
      background-position: 0 0, 0 0;
    }

    /* 3. gradient image */
    .gradient {
      background: linear-gradient(135deg, #ede9fe, #fbcfe8, #fef3c7);
      color: #1e1b3a;
      text-shadow: none;
    }

    /* 4. stacked gradients (sun + sky) */
    .sun {
      background:
        radial-gradient(circle at 30% 80%, #fde68a 0 100px, transparent 200px),
        linear-gradient(to bottom, #1e3a8a, #60a5fa);
    }

    /* 5. tiling (one big image is replaced with a pattern) */
    .tile {
      background-image:
        linear-gradient(45deg, #ede9fe 25%, transparent 25%),
        linear-gradient(-45deg, #fbcfe8 25%, transparent 25%);
      background-size: 30px 30px;
      background-color: #fffbf5;
      color: #1e1b3a;
      text-shadow: none;
    }

    /* 6. overlay text on a gradient */
    .overlay {
      background:
        linear-gradient(rgba(0,0,0,0.4), rgba(0,0,0,0.7)),
        repeating-linear-gradient(45deg, #312e81 0 20px, #1e1b3a 20px 40px);
    }
  </style>
</head>
<body>
  <section class="solid">
    <h2>1. Solid color</h2>
    <p>The simplest background — just one color.</p>
  </section>

  <section class="pattern">
    <h2>2. Pattern with stacked images</h2>
    <p>One radial gradient over one linear gradient — pure CSS polka dots.</p>
  </section>

  <section class="gradient">
    <h2>3. Simple gradient</h2>
    <p>Linear gradient from purple to pink to gold.</p>
  </section>

  <section class="sun">
    <h2>4. Multiple stacked backgrounds</h2>
    <p>A radial sun over a vertical sky.</p>
  </section>

  <section class="tile">
    <h2>5. Tiling pattern</h2>
    <p>Two linear gradients at 45° create a checkered tile.</p>
  </section>

  <section class="overlay">
    <h2>6. Text overlay</h2>
    <p>A dark gradient sits on top of a striped pattern, making text easy to read.</p>
  </section>
</body>
</html>
```

Open it. Six backgrounds, no images — only CSS.

## Common mistakes

- **Forgetting the color fallback** — set a `background-color` under any background image.
- **`background-size: 100% 100%` stretching images weirdly** — usually `cover` or `contain` is what you want.
- **Tile images with default `repeat`** — sometimes it works, often it doesn't. Always check.

## Debugging tips

- In DevTools → Computed, you can see the resolved `background-image` and `background-size`.
- Try a temporary `outline: 4px solid red;` on the element to confirm where the box actually starts/ends.

## Quick quiz

1. What's the difference between `background-size: cover` and `contain`?
2. In stacked backgrounds, which layer is on top?
3. Which property makes a background stay still when you scroll?

<details>
<summary>Answers</summary>

1. `cover` fills the area, cropping edges. `contain` fits inside, leaving gaps.
2. The first one in the comma-separated list.
3. `background-attachment: fixed`.

</details>

## Mini challenge

Recreate the "sun" example but make it a moon. Same CSS, different colors.

## What's next

We can paint the canvas. Now we frame each element — borders.

→ [Lesson 12 — Borders & Rounded Corners](12-borders.md)
