# Lesson 43 — Responsive Design Intro & Viewport

> **The folding-table analogy.** A responsive design is a folding table that **changes shape** to fit the room. On a tiny phone, it's a breakfast nook. On a tablet, it's a 4-seater. On a desktop, it's a banquet table. Same table — different shape.

## What you'll learn

- What "responsive design" really means.
- The viewport meta tag.
- The mobile-first philosophy.
- The 4 building blocks: fluid grids, fluid images, media queries, viewport units.

## Why this matters

If your site doesn't work on phones, you lose half your audience — literally. Responsive is no longer optional.

## What is responsive design?

A responsive page:

- Looks good on a phone (320 – 480 px).
- Looks good on a tablet (768 – 1024 px).
- Looks good on a laptop (1280 – 1920 px).
- Looks good on giant monitors (1920 px+).

Achieve this with:

1. **Fluid grids** — widths in `%` or `fr`, not `px`.
2. **Fluid typography** — `clamp()` or media queries.
3. **Fluid images** — `max-width: 100%; height: auto;`.
4. **Media queries** — different rules at different widths.

## The viewport meta tag

Without this, mobile browsers act like they would on desktop:

```html
<head>
  <meta name="viewport" content="width=device-width, initial-scale=1">
</head>
```

This tells mobile browsers: *render the page at the device's actual width.* Without it, a 1200px layout shows up zoomed out on a phone. **Always include this in the head.**

## The cascade of mobile sizes

Most modern phones are 360 – 414 px wide (CSS pixels).
Tablets are 768 – 1024 px.
Laptops are 1280 px+.

Common breakpoints:

```css
/* mobile-first */
@media (min-width: 640px)  { /* tablet-ish up */ }
@media (min-width: 1024px) { /* laptop-ish up */ }
@media (min-width: 1280px) { /* desktop up    */ }
```

Use `min-width` (mobile-first) — write the small screen's rules in the base, then enhance upward.

## Mobile-first means

```css
/* base = phone */
.card { padding: 12px; }

/* tablets and up */
@media (min-width: 640px) {
  .card { padding: 20px; }
}
```

It's the opposite of the older "desktop first" approach (which used `max-width`). Mobile-first has fewer overrides and feels cleaner.

## Fluid building blocks

### Fluid text

```css
h1 { font-size: clamp(1.8rem, 4vw, 3.5rem); }   /* 1.8rem min, 3.5rem max, grows with vw */
```

### Fluid gaps

```css
.section { padding: clamp(1rem, 4vw, 4rem); }
```

### Fluid images

```css
img { max-width: 100%; height: auto; }
```

### Fluid container

```css
.container { width: min(90%, 1200px); margin-inline: auto; }
```

## Try it yourself

Create `practice/lesson43.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Lesson 43 — responsive intro</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body {
      font-family: system-ui, sans-serif;
      margin: 0;
      background: #fffbf5;
      color: #1e293b;
    }
    .container {
      width: min(90%, 1100px);
      margin-inline: auto;
      padding: clamp(1rem, 4vw, 3rem);
    }

    h1 {
      font-size: clamp(1.8rem, 5vw, 3.5rem);
      line-height: 1.1;
      color: #6d28d9;
      margin: 0 0 16px;
    }
    p { font-size: clamp(1rem, 2vw, 1.2rem); line-height: 1.6; }

    .grid {
      display: grid;
      gap: clamp(0.5rem, 2vw, 1.5rem);
      grid-template-columns: 1fr;        /* mobile default */
      margin-top: 24px;
    }
    @media (min-width: 640px) {
      .grid { grid-template-columns: repeat(2, 1fr); }
    }
    @media (min-width: 1024px) {
      .grid { grid-template-columns: repeat(4, 1fr); }
    }
    .card {
      background: white;
      border-radius: 14px;
      padding: 20px;
      box-shadow: 0 4px 14px rgba(0,0,0,0.08);
    }

    .pill {
      display: inline-block;
      padding: 4px 12px;
      background: #ede9fe;
      border-radius: 999px;
      font-size: 12px;
      color: #5b21b6;
      font-weight: 600;
    }
  </style>
</head>
<body>
  <div class="container">
    <span class="pill">Lesson 43</span>
    <h1>A page that bends, not breaks</h1>
    <p>
      Resize the window. The text grows smoothly, padding grows with the viewport,
      and the grid expands from 1 column on phones to 4 on desktop. All without
      JavaScript.
    </p>

    <div class="grid">
      <div class="card"><h3>Card 1</h3><p>Small screen = 1 col. Big screen = 4 cols.</p></div>
      <div class="card"><h3>Card 2</h3><p>Padding grows from 12 to 32 px.</p></div>
      <div class="card"><h3>Card 3</h3><p>Heading scales with viewport width.</p></div>
      <div class="card"><h3>Card 4</h3><p>Clamp() is the secret sauce.</p></div>
    </div>
  </div>
</body>
</html>
```

Resize the window — the entire page breathes.

## Common mistakes

- **Missing viewport meta tag** — mobile sites appear zoomed out.
- **Using `px` for everything** — looks rigid on small screens.
- **Desktop-first stylesheets** — harder to maintain. Use mobile-first.
- **No `max-width: 100%` on images** — they overflow.

## Debugging tips

- In Chrome DevTools, click the device-toggle icon (top-left, looks like a phone/tablet). Pick a phone, a tablet, see how it looks.

## Quick quiz

1. What's the viewport meta tag and what happens without it?
2. What's the philosophy of mobile-first?
3. Write the rule that makes an image fluid.

<details>
<summary>Answers</summary>

1. `<meta name="viewport" content="width=device-width, initial-scale=1">` — without it, mobile browsers render at desktop width then zoom out.
2. Write styles for mobile, then add `min-width` media queries to enhance for larger screens.
3. `img { max-width: 100%; height: auto; }`

</details>

## Mini challenge

Build a "two-column" layout (text + image) that stacks on mobile but is side by side on desktop. Use `grid-template-columns: 1fr` and switch it at 700px.

## What's next

Mobile-first works — now we learn the actual media-query language.

→ [Lesson 44 — Media Queries](44-media-queries.md)
