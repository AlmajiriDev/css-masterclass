# Lesson 42 — Multiple Columns & Masking

> **The newspaper analogy.** CSS supports true newspaper-style multi-column text — the browser reflows paragraphs across columns automatically. Combined with masks, you can build typography-only effects that used to require Photoshop.

## What you'll learn

- `column-count`, `column-gap`, `column-rule` — newspaper text.
- The CSS `mask` property (linear, radial, image-based).
- `@property` — typed custom properties (animated!).

## Why this matters

Multi-column is fast for newspaper-style text sections. Masks unlock gradients as shapes. `@property` is a brand-new tool that lets custom properties animate smoothly.

## Multi-column text

```css
.article {
  column-count: 2;
  column-gap: 32px;
}
.article p {
  margin: 0 0 16px;
}
```

That's it. The browser splits the paragraph into 2 columns.

```css
.three {
  column-count: 3;
  column-gap: 24px;
}
```

### Custom width instead of count

```css
.flex-cols {
  column-width: 200px;
  column-gap: 16px;
}
```

`column-width` is "as many 200px columns as fit." More flexible than `column-count`.

### Visual divider between columns

```css
.divider {
  column-count: 3;
  column-gap: 24px;
  column-rule: 1px solid #ddd;     /* line between columns */
}
```

### Spanning an element across all columns

```css
.full-width {
  column-span: all;       /* an h2 across all 3 columns */
}
```

### Avoid breaking inside elements

```css
.card {
  break-inside: avoid;   /* don't split this card across columns */
  page-break-inside: avoid; /* legacy */
}
```

## Mask — gradient-based shapes

We saw `mask` briefly. The powerful use is with **gradients**:

```css
.fade-edge {
  -webkit-mask-image: linear-gradient(to bottom, black 80%, transparent 100%);
  mask-image:         linear-gradient(to bottom, black 80%, transparent 100%);
}
```

This makes the bottom of an element fade out — useful for "scroll more" teasers.

```css
.shine {
  position: relative;
}
.shine::after {
  content: "";
  position: absolute;
  inset: 0;
  -webkit-mask: linear-gradient(115deg, transparent 30%, white 50%, transparent 70%);
  background: linear-gradient(115deg, transparent, white, transparent);
}
```

Spotlight shine effect, no JS.

## `@property` — typed custom variables (animated!)

```css
@property --gradient-angle {
  syntax: "<angle>";
  inherits: false;
  initial-value: 0deg;
}

.card {
  --gradient-angle: 0deg;
  background: linear-gradient(var(--gradient-angle), #6d28d9, #f97316);
  transition: --gradient-angle 0.6s;
}
.card:hover {
  --gradient-angle: 90deg;
}
```

Browsers that support `@property` will animate the angle. Without it, custom properties always change **instantly**.

> Note: `@property` is supported in modern browsers. Fallback behavior is no animation, just instant change — still works.

## Try it yourself

Create `practice/lesson42.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 42 — multi-col &amp; masking</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body { font-family: system-ui, sans-serif; padding: 30px; background: #fffbf5; color: #1e293b; max-width: 900px; margin: 0 auto; }
    h2 { color: #1e293b; margin-top: 30px; }

    .news {
      column-count: 3;
      column-gap: 28px;
      column-rule: 1px solid #ddd;
      background: white;
      padding: 24px;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.06);
    }
    .news h2 {
      column-span: all;
      color: #6d28d9;
      text-align: center;
    }
    .news p { margin: 0 0 14px; }

    /* fade-edge mask on a long list */
    .scroll-end {
      max-height: 200px;
      overflow: hidden;
      -webkit-mask-image: linear-gradient(to bottom, black 60%, transparent);
      mask-image:         linear-gradient(to bottom, black 60%, transparent);
      padding: 20px;
      background: white;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.06);
    }

    /* shine on hover */
    .shine {
      position: relative;
      width: 220px;
      padding: 60px 20px;
      border-radius: 12px;
      background: linear-gradient(135deg, #6d28d9, #f97316);
      color: white;
      font-weight: 700;
      text-align: center;
      overflow: hidden;
    }
    .shine::after {
      content: "";
      position: absolute;
      inset: 0;
      background: linear-gradient(115deg, transparent 30%, rgba(255,255,255,0.6) 50%, transparent 70%);
      transform: translateX(-100%);
      transition: transform .6s;
    }
    .shine:hover::after { transform: translateX(100%); }

    /* @property animated angle */
    @property --angle {
      syntax: "<angle>";
      inherits: false;
      initial-value: 0deg;
    }
    .angle-card {
      width: 220px;
      height: 220px;
      background: linear-gradient(var(--angle), #6d28d9, #f97316, #10b981, #6d28d9);
      border-radius: 16px;
      --angle: 0deg;
      transition: --angle 1s ease;
      display: grid; place-items: center;
      color: white;
      font-weight: 700;
      cursor: pointer;
    }
    .angle-card:hover { --angle: 360deg; }
  </style>
</head>
<body>
  <h1>Multi-column &amp; masking</h1>

  <h2>Newspaper layout</h2>
  <div class="news">
    <h2>The CSS Masterclass</h2>
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.</p>
    <p>Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
    <p>Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo.</p>
    <p>Nemo enim ipsam voluptatem quia voluptas sit aspernatur aut odit aut fugit, sed quia consequuntur magni dolores eos qui ratione voluptatem sequi nesciunt.</p>
  </div>

  <h2>Masked scroll-fade</h2>
  <div class="scroll-end">
    <p>Hidden scroll</p>
    <p>Three of cups</p>
    <p>Four of pentacles</p>
    <p>Five of swords</p>
    <p>The tower</p>
    <p>The star</p>
    <p>The moon</p>
    <p>The sun</p>
    <p>Judgement</p>
    <p>The world</p>
  </div>

  <h2>Shine on hover</h2>
  <div class="shine">Hover me</div>

  <h2>Animated gradient angle (@property)</h2>
  <div class="angle-card">Hover for rotating gradient</div>
</body>
</html>
```

Hover the angle card in Chrome, Edge, Safari — see the smooth angle rotation.

## Common mistakes

- **Column text without `break-inside: avoid`** — cards get split mid-content. Use `break-inside: avoid` on cards in column flows.
- **Mask without `-webkit-` prefix** — Safari still needs the prefix.
- **Animating custom properties without `@property`** — instant snap, not smooth.

## Quick quiz

1. Make a 3-column text with a 24px gap and a thin gray rule.
2. What does `column-span: all` do?
3. Why use `@property` instead of just `--my-var: 0deg;`?

<details>
<summary>Answers</summary>

1. `column-count: 3; column-gap: 24px; column-rule: 1px solid #ccc;`
2. The element spans across all columns, not just one.
3. So the browser knows the type (`<angle>`, `<color>`, etc.) and can animate it smoothly. Plain custom properties only swap instantly.

</details>

## Mini challenge

Build a "featured products" 4-column section. Each card has hover shine. Use multi-column only if appropriate; otherwise stay with a grid.

## What's next

Now we enter the responsive part — viewports, mobile-first, media queries.

→ [Lesson 43 — Responsive Design Intro & Viewport](43-responsive.md)
