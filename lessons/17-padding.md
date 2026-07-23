# Lesson 17 — Padding

> **The picture frame analogy.** Padding is the mat around a picture. It separates the picture from the frame and makes it feel intentional. Without padding, everything looks like text and color are screaming at each other.

## What you'll learn

- The `padding` shorthand and per-side padding.
- When to use padding vs margin.
- Logical padding (`padding-inline`, `padding-block`).
- Padding and `display: inline` gotchas.

## Why this matters

Most beginner pages look "cramped" because there's no padding. Add 16-24px to everything and the page feels 10× more polished — that's literally the magic of CSS.

## The shorthand

```css
.box {
  padding: 10px;                /* all 4 sides */
  padding: 10px 20px;           /* vertical horizontal */
  padding: 10px 20px 30px;      /* top  horizontal  bottom */
  padding: 10px 20px 30px 40px; /* top  right     bottom left */
}
```

Memory aid: **TRouBLe** (Top, Right, Bottom, Left).

### Per-side

```css
.box {
  padding-top: 10px;
  padding-right: 20px;
  padding-bottom: 10px;
  padding-left: 20px;
}
```

Use per-side when only one or two need changing.

## Padding vs margin — when to use which

A practical guide:

- **Padding** → space *inside* the box, around its content.
- **Margin** → space *outside*, between this box and others.

```css
.button {
  padding: 12px 24px;       /* inside — bigger = bigger click area */
  margin: 8px 0;            /* outside — keeps buttons apart */
}
```

A button with padding 12px 24px has a nice click zone. Without it, it's just text.

## A common recipe — card

```css
.card {
  padding: 24px;            /* all sides */
  background: white;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.08);
}
```

A 24px padding is a sweet spot for most cards.

## Logical properties — for RTL languages

```css
.box {
  padding-inline: 20px;     /* left in LTR, right in RTL */
  padding-block: 16px;      /* top in both */
}
```

Use these when you expect multi-language support (Arabic, Hebrew). Otherwise regular padding works fine.

## Inline elements and padding

Padding works on inline elements but **only horizontally**:

```css
span { padding: 20px; }          /* vertical padding has no visual effect */
a    { padding: 10px 20px; }     /* works, but no height */

a { display: inline-block; }     /* now padding works on all sides */
```

This is the fix you'll need when making buttons from `<a>` tags.

## Try it yourself

Create `practice/lesson17.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 17 — padding</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body {
      font-family: system-ui, sans-serif;
      padding: 30px;
      background: #fffbf5;
    }

    .row { display: flex; flex-wrap: wrap; gap: 20px; align-items: center; margin-bottom: 30px; }

    .btn {
      background: #6d28d9;
      color: white;
      text-decoration: none;
      border: 0;
      border-radius: 8px;
      cursor: pointer;
      font-size: 16px;
    }
    .btn-a { padding: 4px 8px; }
    .btn-b { padding: 12px 24px; }
    .btn-c { padding: 24px 40px; font-size: 20px; }

    .inline-pad {
      background: #fde68a;
      padding: 10px 16px;       /* works horizontally; vertical visual gap is line-height */
    }

    /* card with consistent padding */
    .card {
      background: white;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.08);
      padding: 24px;
      max-width: 320px;
      margin-bottom: 16px;
    }

    /* per-side padding */
    .per-side {
      background: #ede9fe;
      padding-top: 6px;
      padding-right: 32px;
      padding-bottom: 24px;
      padding-left: 12px;
      max-width: 320px;
    }

    /* logical padding */
    .logical {
      background: #fbcfe8;
      padding-inline: 32px;
      padding-block: 16px;
      max-width: 320px;
    }
  </style>
</head>
<body>
  <h1>Padding</h1>

  <h2>Buttons get bigger with more padding</h2>
  <div class="row">
    <a class="btn btn-a" href="#">Tiny</a>
    <a class="btn btn-b" href="#">Medium</a>
    <a class="btn btn-c" href="#">Big</a>
  </div>

  <h2>Inline padding — only horizontal shows</h2>
  <p>
    Here is some text with an
    <span class="inline-pad">inline yellow span</span>
    in the middle. The vertical padding is invisible.
  </p>

  <h2>Cards</h2>
  <div class="card">
    <h3 style="margin-top:0">Card title</h3>
    <p>24px of padding around the content gives it room to breathe.</p>
  </div>

  <h2>Per-side</h2>
  <div class="per-side">asymmetric padding (TRouBLe)</div>

  <h2>Logical padding</h2>
  <div class="logical">padding-inline + padding-block — same look today, future-proof for RTL.</div>
</body>
</html>
```

## Common mistakes

- **Padding on inline `<a>` and seeing no height** — set `display: inline-block`.
- **`padding: 0` accidentally inherited from a reset** — make sure the card has explicit padding.
- **Padding pushing width past parent's** — solved with `box-sizing: border-box` (Lesson 15).

## Debugging tips

- DevTools → Computed → **Box Model tab**. Each layer is colored. Padding is green.

## Quick quiz

1. What does `padding: 8px 16px` mean?
2. Why does vertical padding on an inline `<a>` not show?
3. How do you make a button link clickable in a big area without changing its inline nature?

<details>
<summary>Answers</summary>

1. 8px top/bottom, 16px left/right.
2. Inline elements don't accept vertical padding visibly (the line-height is what gives vertical breathing room).
3. Set `display: inline-block;` to keep it inline-like, now padding works in all directions.

</details>

## Mini challenge

Build a 3-stat "facts" section: 3 cards side by side, each with a big number (e.g. "120+", "98%", "4.9★") and a small label below. Use only padding to control spacing inside the cards (no separate headlines).

## What's next

Now we put **space between** — margins.

→ [Lesson 18 — Margins](18-margins.md)
