# Lesson 33 — Dropdowns, Image Gallery & Tooltips

> **The treasure-map analogy.** Dropdowns are pop-ups that point in a direction (like a treasure chest hidden in a clue). Galleries are windows showing one piece of art from a collection. Tooltips are whispers that appear when your cursor lingers. CSS handles all three.

## What you'll learn

- How to build a CSS-only dropdown.
- How to make an image gallery (and a lightbox preview).
- How to make tooltips.
- When JavaScript is needed instead of CSS.

## Why this matters

These three patterns appear in nearly every site. Being able to build them with just CSS makes you fast on small projects.

## Dropdowns (CSS-only)

The simplest dropdown uses `:focus-within` or `:hover`:

```html
<div class="dropdown">
  <button class="drop-btn">Menu</button>
  <ul class="drop-menu">
    <li><a href="#">Item 1</a></li>
    <li><a href="#">Item 2</a></li>
    <li><a href="#">Item 3</a></li>
  </ul>
</div>
```

```css
.dropdown { position: relative; display: inline-block; }

.drop-btn {
  padding: 10px 16px;
  background: #6d28d9;
  color: white;
  border: 0;
  border-radius: 8px;
  cursor: pointer;
}

.drop-menu {
  position: absolute;
  top: calc(100% + 6px);
  left: 0;
  margin: 0;
  padding: 6px;
  list-style: none;
  min-width: 160px;
  background: white;
  border-radius: 10px;
  box-shadow: 0 8px 20px rgba(0,0,0,0.12);
  border: 1px solid #eee;

  opacity: 0;
  visibility: hidden;
  transform: translateY(-4px);
  transition: opacity .15s, transform .15s, visibility .15s;
}

.dropdown:focus-within .drop-menu,
.dropdown:hover .drop-menu {
  opacity: 1;
  visibility: visible;
  transform: translateY(0);
}

.drop-menu a {
  display: block;
  padding: 8px 12px;
  border-radius: 6px;
  text-decoration: none;
  color: #1e293b;
}
.drop-menu a:hover { background: #ede9fe; }
```

> **Note:** `:focus-within` makes the menu open with Tab + keyboard — accessible. `:hover` works for mouse but doesn't focus through tab.

### A more accessible pattern

For real apps, use `<details>` + `<summary>` (native dropdown):

```html
<details class="dropdown">
  <summary class="drop-btn">Menu</summary>
  <ul class="drop-menu">
    <li><a href="#">Item 1</a></li>
    <li><a href="#">Item 2</a></li>
  </ul>
</details>
```

```css
.dropdown summary { list-style: none; }
.dropdown summary::-webkit-details-marker { display: none; }
.dropdown[open] .drop-menu { display: block; }
```

The browser handles all keyboard and accessibility for free.

## Image gallery

A grid of thumbnails that opens a fullscreen image:

```html
<div class="gallery">
  <a href="big1.jpg"><img src="thumb1.jpg" alt=""></a>
  <a href="big2.jpg"><img src="thumb2.jpg" alt=""></a>
  …
</div>
```

```css
.gallery {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
  gap: 8px;
}
.gallery img {
  width: 100%;
  height: 200px;
  object-fit: cover;
  display: block;
  border-radius: 8px;
  transition: transform .2s, filter .2s;
  cursor: zoom-in;
}
.gallery img:hover {
  transform: scale(1.02);
  filter: brightness(0.9);
}
```

### Modal preview with the `<dialog>` element (native!)

```html
<dialog id="viewer">
  <img alt="" id="viewerImg">
  <form method="dialog"><button>Close</button></form>
</dialog>
```

```js
document.querySelectorAll('.gallery a').forEach(a => {
  a.addEventListener('click', e => {
    e.preventDefault();
    viewerImg.src = a.href;
    viewer.showModal();
  });
});
```

The `<dialog>` element gives you free ESC-to-close, focus trap, and backdrop. CSS to style the backdrop:

```css
dialog::backdrop {
  background: rgba(0,0,0,0.7);
  backdrop-filter: blur(6px);
}
dialog {
  border: 0;
  padding: 0;
  border-radius: 12px;
  max-width: 90vw;
  max-height: 90vh;
}
```

## Tooltips (pure CSS)

We saw one in Lesson 27. The reusable version:

```html
<button class="tip" data-tip="Click me!">Hover</button>
```

```css
.tip {
  position: relative;
  cursor: help;
  border: 0; background: #ede9fe; padding: 8px 14px; border-radius: 8px;
}
.tip::after {
  content: attr(data-tip);
  position: absolute;
  bottom: calc(100% + 8px);
  left: 50%;
  transform: translateX(-50%);
  background: #1e293b;
  color: white;
  padding: 6px 10px;
  border-radius: 6px;
  white-space: nowrap;
  opacity: 0;
  visibility: hidden;
  transition: opacity .2s;
}
.tip:hover::after,
.tip:focus-visible::after {
  opacity: 1;
  visibility: visible;
}
```

## Try it yourself

Create `practice/lesson33.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 33</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body { font-family: system-ui, sans-serif; padding: 30px; background: #fffbf5; }
    h2 { color: #1e293b; margin-top: 30px; }

    /* drop down */
    .dropdown { position: relative; display: inline-block; }
    .drop-btn { padding: 10px 16px; background: #6d28d9; color: white; border: 0; border-radius: 8px; cursor: pointer; }
    .drop-menu {
      position: absolute;
      top: calc(100% + 6px); left: 0;
      margin: 0; padding: 6px;
      list-style: none; min-width: 200px;
      background: white;
      border-radius: 10px;
      box-shadow: 0 8px 20px rgba(0,0,0,0.12);
      border: 1px solid #eee;
      opacity: 0;
      visibility: hidden;
      transform: translateY(-4px);
      transition: opacity .15s, transform .15s, visibility .15s;
    }
    .dropdown:focus-within .drop-menu,
    .dropdown:hover .drop-menu {
      opacity: 1; visibility: visible; transform: translateY(0);
    }
    .drop-menu a {
      display: block; padding: 8px 12px; border-radius: 6px; text-decoration: none; color: #1e293b;
    }
    .drop-menu a:hover { background: #ede9fe; }

    /* gallery */
    .gallery {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
      gap: 8px;
      max-width: 700px;
    }
    .gallery .tile {
      aspect-ratio: 1/1;
      border-radius: 8px;
      transition: transform .2s, filter .2s;
      cursor: zoom-in;
    }
    .gallery .tile:hover { transform: scale(1.02); filter: brightness(0.9); }

    /* tooltip */
    .tip {
      position: relative;
      cursor: help;
      border: 0;
      background: #ede9fe;
      padding: 8px 14px;
      border-radius: 8px;
    }
    .tip::after {
      content: attr(data-tip);
      position: absolute;
      bottom: calc(100% + 8px);
      left: 50%;
      transform: translateX(-50%);
      background: #1e293b;
      color: white;
      padding: 6px 10px;
      border-radius: 6px;
      white-space: nowrap;
      opacity: 0;
      visibility: hidden;
      transition: opacity .2s;
    }
    .tip:hover::after, .tip:focus-visible::after {
      opacity: 1; visibility: visible;
    }
  </style>
</head>
<body>
  <h1>Dropdowns, galleries, tooltips</h1>

  <h2>Dropdown</h2>
  <div class="dropdown">
    <button class="drop-btn">Actions</button>
    <ul class="drop-menu">
      <li><a href="#">Edit</a></li>
      <li><a href="#">Duplicate</a></li>
      <li><a href="#">Delete</a></li>
    </ul>
  </div>
  <p style="color:#6b7280">Tab into the button and press Enter, or hover.</p>

  <h2>Image gallery (CSS only)</h2>
  <div class="gallery">
    <div class="tile" style="background:linear-gradient(135deg,#6d28d9,#f97316)"></div>
    <div class="tile" style="background:linear-gradient(135deg,#10b981,#0ea5e9)"></div>
    <div class="tile" style="background:linear-gradient(135deg,#f43f5e,#fb7185)"></div>
    <div class="tile" style="background:linear-gradient(135deg,#0ea5e9,#6366f1)"></div>
    <div class="tile" style="background:linear-gradient(135deg,#fbbf24,#f59e0b)"></div>
    <div class="tile" style="background:linear-gradient(135deg,#a855f7,#ec4899)"></div>
  </div>

  <h2>Tooltips</h2>
  <button class="tip" data-tip="Click me!">Hover me</button>
  <button class="tip" data-tip="Help is here!">Another</button>
</body>
</html>
```

## Common mistakes

- **Dropdown that uses `:hover` only** — keyboard users can't reach it.
- **Modal without focus trap** — accessibility fail. Use `<dialog>`.
- **Tooltip covering the element on hover** — set `pointer-events: none` on the tooltip.

## Quick quiz

1. Why prefer `<details>` over `:focus-within` for a dropdown?
2. What's the modern HTML element for modals?
3. What does `dialog::backdrop` style?

<details>
<summary>Answers</summary>

1. It's keyboard-accessible and accessible by default. No fancy CSS needed.
2. `<dialog>` (with `showModal()`).
3. The dimmed area behind a modal `<dialog>`.

</details>

## Mini challenge

Build a `<details>`-based menu that has 3 sub-items each, and confirm Tab + Enter opens it.

## What's next

We've finished the components block. Now we add motion — first, transforms (move, scale, rotate, skew).

→ [Lesson 34 — Transforms (2D & 3D)](34-transforms.md)
