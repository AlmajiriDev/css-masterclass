# Lesson 22 — Z-index & Stacking

> **The stacking-paper analogy.** When you place papers on a desk, the one on top is what you see. CSS has a stacking order (z-index) that decides who's on top. Higher numbers win. Tie-break rules decide equal numbers. Some elements ignore the stack entirely (the dreaded "my z-index does nothing" bug).

## What you'll learn

- How the stacking order works.
- When z-index applies — and when it doesn't.
- `position: relative` + `z-index` for layering.
- Stacking contexts (the most-confusing-but-important idea).

## Why this matters

If you've ever had a dropdown menu hide behind another element, or a modal go behind the overlay, you needed z-index. Knowing stacking prevents hours of confusion.

## How z-index works

`z-index` only applies to elements that are **positioned** (`relative`, `absolute`, `fixed`, or `sticky`) — or to flex/grid items.

```css
.modal    { position: fixed; z-index: 100; }
.backdrop { position: fixed; z-index: 50;  }
```

The modal sits above the backdrop.

### Rules

- **Higher z-index wins.**
- **Equal z-index** → later in the HTML wins.
- **Negative** z-index puts the element **behind** its parent.
- **Default z-index** is `auto` — no manual stacking.

## Stacking contexts (the bug behind bugs)

A stacking context is a group of elements whose z-indexes are compared **inside** it, not with the outside world.

An element forms a new stacking context when:

- `position` is non-static **and** `z-index` is non-auto
- `opacity` is less than `1`
- `transform` is set to anything other than `none`
- `filter` or `backdrop-filter` is set
- `isolation: isolate` (an explicit way)

This matters because:

```css
.parent { position: relative; z-index: 1; }    /* new stacking context */
.child  { position: absolute; z-index: 9999; } /* stranded inside parent */
```

Even with `z-index: 9999`, the child can't escape the parent's stacking context. The parent itself sits at `z-index: 1` in the global order.

## A fix: `isolation: isolate`

```css
.sidebar { isolation: isolate; }   /* makes a clean stacking context */
```

This is a no-op visually but it puts the sidebar's z-indices into a tidy local group.

## Common patterns

### Modal overlay

```css
.overlay {
  position: fixed; inset: 0;
  background: rgba(0,0,0,0.5);
  z-index: 100;
}
.modal {
  position: fixed;
  top: 50%; left: 50%;
  transform: translate(-50%, -50%);
  z-index: 110;
}
```

### Tooltip above everything

```css
.tooltip { position: relative; }
.tooltip::after {
  content: "Hi!";
  position: absolute;
  bottom: 100%;
  left: 50%;
  transform: translateX(-50%);
  background: #1e293b;
  color: white;
  padding: 6px 10px;
  border-radius: 6px;
  z-index: 10;
}
```

### A "tooltip going behind a card" bug, and the fix

```html
<div class="card">
  <div class="tooltip">?</div>
</div>
<div class="next-card"></div>
```

If `.card` has `transform: scale(1)` (or any transform), it forms a stacking context. The tooltip inside can't escape `.card`. Either:

1. Move the tooltip out of the transformed parent, or
2. Set `z-index: 1` on `.card` and rely on DOM order, or
3. Avoid `transform` on the parent if not necessary.

## Try it yourself

Create `practice/lesson22.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 22 — z-index</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body {
      font-family: system-ui, sans-serif;
      padding: 30px;
      background: #fffbf5;
    }

    .stage {
      position: relative;
      width: 320px; height: 220px;
      margin: 0 auto 40px;
      background: #f1f5f9;
      border-radius: 12px;
    }
    .box {
      width: 120px; height: 80px;
      padding: 12px;
      border-radius: 8px;
      color: white;
      font-weight: 600;
      box-shadow: 0 4px 12px rgba(0,0,0,0.15);
    }
    .a {
      background: #f97316;
      position: absolute; top: 20px; left: 20px;
      z-index: 1;
    }
    .b {
      background: #6d28d9;
      position: absolute; top: 60px; left: 80px;
      z-index: 2;       /* on top */
    }
    .c {
      background: #059669;
      position: absolute; top: 100px; left: 140px;
      z-index: 3;       /* on top of everything */
    }

    /* stacking context trap */
    .trap {
      display: flex;
      gap: 20px;
      margin-bottom: 30px;
    }
    .parent {
      position: relative;
      width: 160px; height: 80px;
      background: #ede9fe;
      border-radius: 8px;
      padding: 12px;
    }
    /* transform on parent creates a stacking context */
    .parent.transformed {
      transform: rotate(0deg);   /* even rotate(0) creates one */
    }
    .kid {
      position: absolute;
      top: -10px; right: -10px;
      background: #dc2626;
      color: white;
      padding: 4px 8px;
      border-radius: 999px;
      z-index: 9999;   /* stranded */
    }
    /* non-transformed parent: kid escapes properly */
    .clean-parent .kid { background: #059669; }

    /* modal demo */
    .modal-page {
      position: relative;
      height: 200px;
      background: linear-gradient(135deg, #1e3a8a, #6d28d9);
      border-radius: 12px;
      overflow: hidden;
    }
    .modal-overlay {
      position: absolute; inset: 0;
      background: rgba(0,0,0,0.5);
      z-index: 100;
    }
    .modal-card {
      position: absolute;
      top: 50%; left: 50%;
      transform: translate(-50%, -50%);
      background: white;
      padding: 20px 30px;
      border-radius: 8px;
      z-index: 110;
      color: #1e293b;
    }
  </style>
</head>
<body>
  <h1>z-index</h1>

  <h2>Three stacked boxes (orange, purple, green)</h2>
  <div class="stage">
    <div class="box a">A · z-index 1</div>
    <div class="box b">B · z-index 2</div>
    <div class="box c">C · z-index 3</div>
  </div>
  <p style="text-align:center">C is on top because it has the highest z-index.</p>

  <h2>Stacking context trap</h2>
  <div class="trap">
    <div class="parent">
      parent A (plain)
      <span class="kid">9999</span>
    </div>
    <div class="parent transformed">
      parent B (has transform)
      <span class="kid">9999</span>
    </div>
  </div>
  <p style="text-align:center">
    Both kids have z-index 9999. The transformed parent's stacking context traps its kid — it
    can only escape if the parent itself sits above other contexts.
  </p>

  <h2>Modal pattern</h2>
  <div class="modal-page">
    <div class="modal-overlay"></div>
    <div class="modal-card">A modal sits above its overlay with z-index 110 vs 100.</div>
  </div>
</body>
</html>
```

## Common mistakes

- **`z-index: 99999` on something and it still hides** — check if a parent is making a stacking context.
- **Negative z-index on inline elements** — doesn't apply. They must be positioned.
- **Mixing `z-index: 0` and `z-index: auto`** — `0` is a real z-index that creates a stacking context. `auto` doesn't.

## Debugging tips

- In Chrome DevTools → **Layers panel**, see exactly which elements form stacking contexts.
- In DevTools → Computed, look for the **3D view** button — it shows z-order visually.

## Quick quiz

1. Does z-index work on a static element?
2. A child has `z-index: 9999` but is hidden behind another element. What's the first thing to check?
3. What's the safest way to create a stacking context on purpose?

<details>
<summary>Answers</summary>

1. No. The element must be positioned or be a flex/grid item.
2. Whether any **ancestor** creates a stacking context that traps it.
3. `isolation: isolate;`

</details>

## Mini challenge

Build a layered "card with overlay badge" pattern — like a profile card with a "PRO" badge in the top-right corner — and verify the badge always shows above the card content.

## What's next

Now we look at the older CSS layout systems — float, inline-block — that birthed modern layout.

→ [Lesson 23 — Float & Inline-block](23-float-inline-block.md)
