# Lesson 32 — Buttons & Cards

> **The LEGO analogy.** Cards are like Lego bricks with content inside. Buttons are the studs you press to make things happen. Together, they're how 90% of modern web UIs are built.

## What you'll learn

- How to style a button properly (and not use `<div>` for buttons).
- Button variations: primary, secondary, ghost, icon.
- Card anatomy: image, header, body, footer, actions.
- Card hover effects that feel "alive".

## Why this matters

You'll click a button 1000 times today and 1000 times tomorrow. Whether that click feels good depends on CSS. Cards show data in every modern app — learn them well.

## Buttons

### The HTML rule

Use `<button>` for actions (form submit, toggle, open dialog) and `<a>` for navigation. Always.

```html
<button type="submit">Save</button>
<a class="btn" href="/home">Home</a>
```

### Base button style

```css
button, .btn {
  display: inline-block;
  font: inherit;
  cursor: pointer;
  text-decoration: none;
  border: 1px solid transparent;
  border-radius: 8px;
  padding: 10px 18px;
  font-weight: 600;
  line-height: 1.2;
  transition: background .15s, transform .1s, box-shadow .15s;
}
button:disabled,
.btn[aria-disabled="true"] {
  opacity: 0.5;
  cursor: not-allowed;
}
```

### Variations

```css
.primary   { background: #6d28d9; color: white; }
.primary:hover { background: #5b21b6; }
.primary:active { transform: translateY(1px); }

.secondary { background: white; color: #1e293b; border-color: #d1d5db; }
.secondary:hover { background: #f5f5f5; }

.ghost     { background: transparent; color: #6d28d9; }
.ghost:hover { background: #ede9fe; }

.danger    { background: #dc2626; color: white; }
.danger:hover { background: #b91c1c; }
```

### Icon button

```css
.icon-btn {
  width: 40px;
  height: 40px;
  padding: 0;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
}
```

### Sizes

```css
.sm { padding: 6px 12px; font-size: 14px; }
.md { padding: 10px 18px; font-size: 16px; }   /* default */
.lg { padding: 14px 28px; font-size: 18px; }
```

### Touch-friendly

A touch target should be **at least 44×44 px** (Apple HIG) or 48×48 (Material). Use `min-width: 44px; min-height: 44px;` if your padding is small.

## Cards

A card groups content visually. Anatomy: media (image), title, body, footer/actions.

```html
<article class="card">
  <div class="card-media">
    <img alt="" src="...">
  </div>
  <div class="card-body">
    <h3 class="card-title">Title</h3>
    <p>Description…</p>
  </div>
  <div class="card-footer">
    <button class="primary">Save</button>
  </div>
</article>
```

### Base card

```css
.card {
  background: white;
  border-radius: 14px;
  overflow: hidden;
  box-shadow: 0 4px 14px rgba(0,0,0,0.08);
  display: flex;
  flex-direction: column;
  max-width: 320px;
}
.card-media { aspect-ratio: 16/9; overflow: hidden; background: #ddd; }
.card-media img { width: 100%; height: 100%; object-fit: cover; display: block; }
.card-body { padding: 16px 18px; flex: 1; }
.card-title { margin: 0 0 6px; font-size: 18px; }
.card-footer {
  padding: 12px 18px;
  border-top: 1px solid #eee;
  display: flex;
  gap: 8px;
}
```

### Hover lift

```css
.card {
  transition: transform .2s, box-shadow .2s;
}
.card:hover {
  transform: translateY(-4px);
  box-shadow: 0 12px 24px rgba(0,0,0,0.14);
}
```

## Try it yourself

Create `practice/lesson32.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 32 — buttons &amp; cards</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body {
      font-family: system-ui, sans-serif;
      background: #fffbf5;
      padding: 30px;
      margin: 0;
    }

    /* buttons */
    button, .btn {
      display: inline-block;
      font: inherit;
      cursor: pointer;
      text-decoration: none;
      border: 1px solid transparent;
      border-radius: 8px;
      padding: 10px 18px;
      font-weight: 600;
      line-height: 1.2;
      transition: background .15s, transform .1s, box-shadow .15s;
    }
    button:focus-visible, .btn:focus-visible { outline: 3px solid #fde68a; outline-offset: 2px; }

    .primary    { background: #6d28d9; color: white; }
    .primary:hover  { background: #5b21b6; }
    .primary:active { transform: translateY(1px); }
    .secondary  { background: white; color: #1e293b; border-color: #d1d5db; }
    .ghost      { background: transparent; color: #6d28d9; }
    .ghost:hover { background: #ede9fe; }
    .danger     { background: #dc2626; color: white; }

    .sm { padding: 6px 12px; font-size: 14px; }
    .lg { padding: 14px 28px; font-size: 18px; }
    .icon-btn {
      width: 40px; height: 40px;
      padding: 0;
      display: inline-flex; align-items: center; justify-content: center;
      border-radius: 50%;
    }

    /* cards */
    h2 { margin-top: 40px; color: #1e293b; }

    .grid {
      display: grid;
      gap: 20px;
      grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
      max-width: 920px;
    }

    .card {
      background: white;
      border-radius: 14px;
      overflow: hidden;
      box-shadow: 0 4px 14px rgba(0,0,0,0.08);
      display: flex;
      flex-direction: column;
      transition: transform .2s, box-shadow .2s;
    }
    .card:hover { transform: translateY(-4px); box-shadow: 0 12px 24px rgba(0,0,0,0.14); }
    .card-media {
      aspect-ratio: 16/9;
      background: linear-gradient(135deg, #6d28d9, #f97316);
    }
    .card-body { padding: 16px 18px; flex: 1; }
    .card-title { margin: 0 0 6px; font-size: 18px; }
    .card-footer {
      padding: 12px 18px;
      border-top: 1px solid #eee;
      display: flex; gap: 8px;
    }

    .row { display: flex; gap: 8px; flex-wrap: wrap; align-items: center; margin-bottom: 16px; }
  </style>
</head>
<body>
  <h1>Buttons &amp; cards</h1>

  <h2>Button variations</h2>
  <div class="row">
    <button class="primary">Primary</button>
    <button class="secondary">Secondary</button>
    <button class="ghost">Ghost</button>
    <button class="danger">Danger</button>
    <button class="primary" disabled>Disabled</button>
  </div>

  <h2>Sizes</h2>
  <div class="row">
    <button class="primary sm">Small</button>
    <button class="primary">Default</button>
    <button class="primary lg">Large</button>
  </div>

  <h2>Icon button</h2>
  <div class="row">
    <button class="primary icon-btn" aria-label="Add">+</button>
    <button class="ghost   icon-btn" aria-label="Settings">⚙</button>
    <button class="danger  icon-btn" aria-label="Delete">🗑</button>
  </div>

  <h2>Cards</h2>
  <div class="grid">
    <article class="card">
      <div class="card-media" style="background:linear-gradient(135deg,#6d28d9,#f97316)"></div>
      <div class="card-body">
        <h3 class="card-title">Sunset</h3>
        <p>A warm gradient card with hover lift.</p>
      </div>
      <div class="card-footer">
        <button class="primary sm">Save</button>
        <button class="ghost sm">Share</button>
      </div>
    </article>

    <article class="card">
      <div class="card-media" style="background:linear-gradient(135deg,#10b981,#0ea5e9)"></div>
      <div class="card-body">
        <h3 class="card-title">Forest</h3>
        <p>Animation on hover. Cool gradient.</p>
      </div>
      <div class="card-footer">
        <button class="primary sm">Open</button>
      </div>
    </article>

    <article class="card">
      <div class="card-media" style="background:linear-gradient(135deg,#f43f5e,#fb7185)"></div>
      <div class="card-body">
        <h3 class="card-title">Rose</h3>
        <p>Hover the cards to see the lift effect.</p>
      </div>
      <div class="card-footer">
        <button class="primary sm">Try</button>
      </div>
    </article>
  </div>
</body>
</html>
```

Hover the cards. Click the buttons. Notice the press effect on primary.

## Common mistakes

- **Using `<div>` for a clickable thing** — no keyboard support. Use `<button>` (or `<a>` if it goes somewhere).
- **Disabled buttons that look the same as enabled** — always style with `opacity` or color change.
- **Cards without a clear hierarchy** — title, then body, then footer. Use type size to make it obvious.
- **Touch targets too small on mobile** — buttons should be at least 44×44 px.

## Quick quiz

1. What's the difference between `<button>` and `<a class="btn">` semantically?
2. What's the minimum recommended touch target size?
3. How do you make a card lift on hover?

<details>
<summary>Answers</summary>

1. `<button>` does an action; `<a>` navigates somewhere.
2. 44×44 px (Apple) or 48×48 px (Google).
3. `transform: translateY(-4px); box-shadow: deeper;` plus `transition`.

</details>

## Mini challenge

Build a "pricing card" component:
- 3 cards side by side.
- Each has a tier name, price, description, a feature list, and a CTA button.
- The middle card should be highlighted (scale, shadow, accent color).

## What's next

Now menus, galleries, and tooltips — interactive components.

→ [Lesson 33 — Dropdowns, Image Gallery & Tooltips](33-dropdowns-tooltips.md)
