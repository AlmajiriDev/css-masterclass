# Lesson 24 — Centering & Alignment

> **The dartboard analogy.** Centering in CSS has 3 concentric rings: easy (text & blocks), medium (absolute positioning), and epic (everything else). Knowing which trick to reach for is half the battle.

## What you'll learn

- How to center text, blocks, and absolutely-positioned elements.
- The "Flexbox centers anything" rule of thumb.
- The "Grid place-self centers anything" alternative.
- Common alignment pitfalls.

## Why this matters

Every project needs centering. Once you know the 3 patterns, you'll never fight it again.

## The 3 rings of centering

### Ring 1 — Easy: text & inline elements

```css
.text  { text-align: center; }       /* text inside a block */
.link  { display: inline-block; }    /* needed if you want padding */
```

That's it for inline content.

### Ring 1 — Easy: a block with known width

```css
.box {
  width: 300px;
  margin: 0 auto;       /* left & right auto centers */
}
```

The block needs a definite width (or `max-width`). `margin: 0 auto` only centers horizontally.

### Ring 2 — Flexbox (the everything-center)

```css
.parent {
  display: flex;
  justify-content: center;   /* horizontal */
  align-items: center;       /* vertical */
}
```

Yes, this is the entire trick. Flexbox makes centering almost trivially easy. We'll cover Flexbox deeply in Lesson 38.

```css
.card {
  display: flex;
  align-items: center;       /* vertical */
  gap: 12px;
}
```

### Ring 2 — Grid (even shorter)

```css
.parent {
  display: grid;
  place-items: center;       /* shorthand for justify-items + align-items */
}
```

`place-items: center` centers in both directions. That's the whole grid centering rule.

### Ring 2 — The "absolute transform" trick

```css
.modal {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

The transform pulls it back by half its own size. Works without needing a flex/grid parent.

A modern variant:

```css
.modal {
  position: absolute;
  inset: 0;             /* fill the parent */
  margin: auto;         /* works with inset:0 to center */
}
```

### Ring 3 — vertical centering without flex/grid (legacy)

```css
.parent { line-height: 200px; text-align: center; }
.child  { display: inline-block; line-height: normal; vertical-align: middle; }
```

Forget this. Use Flexbox. We only mention it so you recognize it.

## Centering variations

### One item horizontally

```css
.center-x {
  margin-inline: auto;    /* width needed */
  width: max-content;     /* shrink to content */
}
```

### Sticky footer (always at bottom)

```css
body { display: flex; flex-direction: column; min-height: 100vh; }
main { flex: 1; }     /* takes all available space */
footer { /* normal height */ }
```

### Form input alignment

```css
form { display: grid; gap: 12px; justify-items: center; }
```

## A common trap — centering inside a transformed parent

If parent has `transform`, the child's `position: fixed` becomes relative to the parent, not the viewport. Surprise!

```css
.parent { transform: translateZ(0); }
.child  { position: fixed; }   /* not actually relative to viewport */
```

Avoid `transform` on full-page ancestors you don't need it for.

## Try it yourself

Create `practice/lesson24.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 24 — centering</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body {
      font-family: system-ui, sans-serif;
      background: #fffbf5;
      padding: 30px;
    }
    h2 { margin-top: 40px; }

    /* 1. text-align */
    .text-demo {
      background: #fde68a;
      padding: 20px;
      text-align: center;
      margin-bottom: 12px;
    }

    /* 2. block with auto margin */
    .block-demo {
      width: 200px;
      margin: 0 auto;
      background: #ede9fe;
      padding: 20px;
      text-align: center;
    }

    /* 3. flex center */
    .flex-demo {
      display: flex;
      justify-content: center;
      align-items: center;
      width: 240px;
      height: 120px;
      background: #ddd6fe;
      border-radius: 8px;
      margin: 12px auto;
    }
    .inner { background: white; padding: 12px 24px; border-radius: 6px; }

    /* 4. grid place-items */
    .grid-demo {
      display: grid;
      place-items: center;
      width: 240px;
      height: 120px;
      background: #fbcfe8;
      border-radius: 8px;
      margin: 12px auto;
    }

    /* 5. absolute transform */
    .abs-demo {
      position: relative;
      width: 320px;
      height: 160px;
      background: #f1f5f9;
      border-radius: 8px;
      margin: 0 auto;
    }
    .abs-inner {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background: white;
      padding: 16px 24px;
      border-radius: 6px;
    }
  </style>
</head>
<body>
  <h1>Centering 5 ways</h1>

  <h2>1. text-align: center</h2>
  <div class="text-demo">Hello, I'm centered.</div>

  <h2>2. block + margin auto</h2>
  <div class="block-demo">I'm 200px wide and centered.</div>

  <h2>3. Flexbox</h2>
  <div class="flex-demo">
    <div class="inner">flex centers me</div>
  </div>

  <h2>4. Grid place-items: center</h2>
  <div class="grid-demo">
    <div class="inner">grid center</div>
  </div>

  <h2>5. Absolute + transform</h2>
  <div class="abs-demo">
    <div class="abs-inner">absolute center</div>
  </div>
</body>
</html>
```

## Common mistakes

- **`margin: auto` on something with no width** — doesn't center. Give it `width` or `max-width`.
- **Vertical `margin: auto` without `flex/grid`** — does nothing.
- **Forgetting the parent's `display: flex`** — child sits in default position.

## Debugging tips

- If something isn't centered, check: does the parent have flex/grid? Does the element have a definite size?

## Quick quiz

1. Center a 500px-wide `<div>` horizontally without flex/grid.
2. Center a single child both horizontally and vertically using the shortest CSS rule.
3. Why does `margin: auto` not center vertically by default?

<details>
<summary>Answers</summary>

1. `width: 500px; margin: 0 auto;`
2. `display: grid; place-items: center;` on the parent.
3. Block layout flows vertically with no defined available height to center within — `auto` resolves to 0 vertically.

</details>

## Mini challenge

Build a "loading" full-screen overlay: a centered spinner in the middle of the screen, behind no other content. Use `position: fixed` + flex centering.

## What's next

Centering covered, now images — how to fit them, filter them, and even shape them.

→ [Lesson 25 — Image Centering, Filters & Shapes](25-images.md)
