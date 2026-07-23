# Lesson 46 — Responsive Images, Video & Container Queries

> **The letterbox analogy.** A movie plays correctly on every TV by letterboxing the content — adding black bars so the aspect ratio stays consistent. CSS does the same for our media. Different screens, same proportion, always sharp.

## What you'll learn

- Responsive images: `srcset` and the `<picture>` element.
- The `loading="lazy"` attribute.
- The aspect-ratio CSS property.
- Embedding responsive videos.
- Container queries for components.

## Why this matters

Big images on phones are slow and waste battery. The phone doesn't need a 4000px-wide hero. CSS and HTML together give the right image to the right device.

## Modern HTML — `<img srcset>` and `sizes`

```html
<img
  alt=""
  src="hero-800.jpg"
  srcset="
    hero-400.jpg 400w,
    hero-800.jpg 800w,
    hero-1600.jpg 1600w
  "
  sizes="(max-width: 600px) 100vw, 50vw"
/>
```

The browser picks the right size:

- A 400px-wide phone → loads `hero-400.jpg`.
- A 1600px-wide desktop → loads `hero-1600.jpg` (if it picks 800w slot, `hero-800.jpg`).

The `sizes` attribute is the **hint** about how big the image will be displayed. Browsers without `srcset` fall back to `src`.

## Art direction — `<picture>`

Same image, different crops:

```html
<picture>
  <source media="(max-width: 600px)" srcset="cropped.jpg">
  <source media="(min-width: 601px)" srcset="wide.jpg">
  <img src="wide.jpg" alt="">
</picture>
```

For hero photo crops, this is the right tool.

## Lazy loading

```html
<img src="..." alt="" loading="lazy" decoding="async">
```

The browser waits to load images until they're about to scroll into view. Saves bandwidth on long pages.

## Aspect ratio — the right one

```css
.media {
  aspect-ratio: 16 / 9;
  width: 100%;
}
img, video {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
```

`aspect-ratio` is the modern replacement for the "padding-bottom hack." Use it on any box that should hold a fixed ratio.

## Responsive video — 16:9 always

```html
<div class="video">
  <iframe src="https://www.youtube.com/embed/xyz" allowfullscreen></iframe>
</div>
```

```css
.video {
  position: relative;
  width: 100%;
  aspect-ratio: 16 / 9;
}
.video iframe {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  border: 0;
}
```

Same trick for any embed.

## Responsive SVG

SVG by default scales infinitely. Set the size with CSS:

```html
<svg viewBox="0 0 100 100">…</svg>
```

```css
.icon { width: 24px; height: 24px; }
.icon.lg { width: 48px; height: 48px; }
```

## Container queries — recap (from Lesson 44)

```css
.card {
  container-type: inline-size;
  container-name: profile;
}

@container profile (min-width: 350px) {
  .card { display: grid; grid-template-columns: 80px 1fr; gap: 16px; }
}
```

This lets a card respond to **its** width, not the viewport. Very useful for reusable components.

## `aspect-ratio` + `object-fit` — the ultimate image recipe

```css
.cover {
  width: 100%;
  aspect-ratio: 16 / 9;
  background: #eee;
  overflow: hidden;
  border-radius: 12px;
}
.cover img {
  width: 100%; height: 100%;
  object-fit: cover;
  display: block;
}
```

## Try it yourself

Create `practice/lesson46.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Lesson 46 — responsive media</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body { font-family: system-ui, sans-serif; padding: 30px; background: #fffbf5; color: #1e293b; max-width: 800px; margin: 0 auto; }
    h2 { color: #1e293b; }

    .cover {
      width: 100%;
      aspect-ratio: 16/9;
      background: #ddd;
      overflow: hidden;
      border-radius: 12px;
    }
    .cover > * {
      width: 100%; height: 100%;
      object-fit: cover;
      display: block;
    }

    /* container query */
    .wrap {
      container-type: inline-size;
      container-name: card;
      background: white;
      padding: 12px;
      border-radius: 12px;
      margin: 16px 0;
    }
    .inner {
      display: grid; gap: 12px;
      grid-template-columns: 1fr;
    }
    @container card (min-width: 400px) {
      .inner { grid-template-columns: 80px 1fr; align-items: center; }
    }
    .avatar {
      width: 60px; height: 60px;
      background: linear-gradient(135deg,#6d28d9,#f97316);
      border-radius: 50%;
    }
    @container card (min-width: 400px) {
      .avatar { width: 80px; height: 80px; }
    }
  </style>
</head>
<body>
  <h1>Responsive media</h1>

  <h2>SVG illustrations (infinite scale)</h2>
  <div class="cover">
    <svg viewBox="0 0 200 112" xmlns="http://www.w3.org/2000/svg" preserveAspectRatio="xMidYMid slice">
      <defs>
        <linearGradient id="g" x1="0" y1="0" x2="1" y2="1">
          <stop offset="0" stop-color="#6d28d9"/>
          <stop offset="1" stop-color="#f97316"/>
        </linearGradient>
      </defs>
      <rect width="100%" height="100%" fill="url(#g)"/>
      <circle cx="160" cy="40" r="20" fill="#fde68a"/>
    </svg>
  </div>

  <h2>Image with `loading="lazy"` (below)</h2>

  <h2>Video container (16:9)</h2>
  <div class="cover" style="background:#0f172a;">
    <div style="display:grid;place-items:center;color:white;font-size:24px;">16:9 container — embed any iframe here</div>
  </div>

  <h2>Container query — card adapts to its width</h2>
  <div class="wrap">
    <div class="inner">
      <div class="avatar"></div>
      <div>
        <strong>Hauwa</strong>
        <p style="margin: 4px 0; color:#6b7280">Container query magic</p>
      </div>
    </div>
  </div>
  <p style="color:#6b7280">Resize the white wrap — the card switches layout at 400px wide.</p>
</body>
</html>
```

## Common mistakes

- **Missing `sizes`** with `srcset` — browser picks a worse fit.
- **Aspect ratio off because of `width: 100%` + no height** — use `aspect-ratio`.
- **Forgetting `object-fit: cover`** — image looks stretched.

## Quick quiz

1. What's the difference between `srcset` and `<picture>`?
2. What attribute makes a phone skip loading an image until it's near the screen?
3. What's the modern way to give a media box a fixed aspect ratio?

<details>
<summary>Answers</summary>

1. `srcset` swaps the file for size. `<picture>` swaps the file for art direction (different crops / formats).
2. `loading="lazy"`.
3. `aspect-ratio: 16 / 9;` on the box.

</details>

## Mini challenge

Build a "video gallery" of 4 placeholder 16:9 thumbnails in a responsive grid. Each thumbnail is a colored SVG with a play icon overlay.

## What's next

Now we get organized and professional — variables and architecture.

→ [Lesson 47 — CSS Variables & @property](47-variables.md)
