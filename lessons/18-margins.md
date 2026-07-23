# Lesson 18 — Margins

> **The personal-space bubble analogy.** Padding is the bubble wrap *inside* your box. Margin is the **personal space** outside — the gap between you and the next person on the elevator. We use it to keep things from touching, to align them, and occasionally to overlap them.

## What you'll learn

- The `margin` shorthand and per-side margin.
- `margin: auto` for centering.
- Negative margins (overlapping elements).
- Margin collapse (the rule we saw in Lesson 15).
- Logical `margin-inline` and `margin-block`.

## Why this matters

Margins separate things. Auto-margin centers things. Negative margins overlap things. Knowing these three tricks solves 80% of CSS spacing problems.

## The shorthand (same as padding)

```css
.box { margin: 20px; }                 /* all sides */
.box { margin: 10px 20px; }            /* vertical horizontal */
.box { margin: 10px 20px 30px 40px; }  /* TRBL */
```

Memory aid: **TRouBLe**.

## Per-side

```css
.card {
  margin-top: 20px;
  margin-right: 0;
  margin-bottom: 20px;
  margin-left: 0;
}
```

## `margin: auto` — centering

The classic centering trick:

```css
.box {
  width: 300px;
  margin-left: auto;
  margin-right: auto;
}
```

- Block element with a defined width gets centered horizontally.
- For vertical centering, use Flexbox or absolute positioning. `margin: auto` doesn't center vertically by default.

**Centering with `max-width`** (the modern way):

```css
.container {
  max-width: 800px;
  margin-inline: auto;       /* centers horizontally */
  padding: 20px;
}
```

## Auto margins and Flexbox

In Flexbox, auto margins have extra powers:

```css
.row {
  display: flex;
}
.item-a { margin-right: auto; }   /* push all later items to the right edge */
```

We'll use this in Lesson 39 for layouts like "logo on the left, menu on the right".

## Negative margins

```css
.overlap { margin-top: -20px; }   /* pulls up by 20px, overlapping the element above */
```

Useful for:

- Tight designs where elements deliberately overlap (e.g., a photo banner poking out from under its container).
- Tight typographic layouts.
- Adjusting grid gaps when a child needs to extend past its parent.

## Margin collapse — quick recap

Two adjacent **vertical** margins combine into the bigger one:

```html
<p>One</p>
<p>Two</p>
```

```css
p { margin: 20px; }     /* gap is 20, not 40 */
```

This is the **default** and usually fine. To kill the collapse (already shown in Lesson 15): add `padding`, `border`, or `overflow: hidden` to the parent.

## The "auto gap" problem

If you want a 16px gap between boxes and used `display: block` (the default), `margin` is the only way:

```css
.card + .card { margin-top: 16px; }    /* every card after the first */
```

`+` is the adjacent sibling combinator from Lesson 05.

## Try it yourself

Create `practice/lesson18.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 18 — margins</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body {
      font-family: system-ui, sans-serif;
      background: #fffbf5;
      margin: 0;
      padding: 30px;
    }

    .centered {
      width: 90%;
      max-width: 600px;
      margin: 30px auto;
      padding: 20px;
      background: #ede9fe;
      border-radius: 12px;
    }

    .row { display: flex; gap: 10px; }
    .grow { margin-left: auto; background: #f97316; color: white; padding: 8px 12px; border-radius: 8px; }
    .other { background: #6d28d9; color: white; padding: 8px 12px; border-radius: 8px; }

    .stack p {
      margin: 20px;
      background: #fef3c7;
      padding: 10px;
    }
    .no-collapse {
      overflow: hidden;
      background: #dcfce7;
      padding: 8px;
    }
    .no-collapse p {
      margin: 20px;
      background: white;
      padding: 10px;
    }

    .photo-banner {
      width: 80%;
      height: 120px;
      background: linear-gradient(135deg, #6d28d9, #f97316);
      margin: 0 auto;
      border-radius: 12px;
    }
    .overlap-card {
      width: 60%;
      margin: -40px auto 0;
      background: white;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 6px 18px rgba(0,0,0,0.15);
      position: relative;     /* we'll explain in Lesson 21 */
    }
  </style>
</head>
<body>
  <h1>Margins</h1>

  <h2>Centered container</h2>
  <div class="centered">max-width + margin: auto — the classic pattern.</div>

  <h2>Margin-left: auto in a Flex row</h2>
  <div class="row">
    <div class="other">Left</div>
    <div class="grow">I'm right-aligned via auto margin</div>
  </div>

  <h2>Margin collapse demo</h2>
  <div class="stack">
    <p>margin 20px</p>
    <p>margin 20px</p>
  </div>
  <p style="color:#6b7280">Gap above = 20px. Margins collapsed.</p>

  <div class="no-collapse">
    <p>margin 20px</p>
    <p>margin 20px</p>
  </div>
  <p style="color:#6b7280">Gap above = 40px because parent has overflow:hidden.</p>

  <h2>Negative margin (overlap)</h2>
  <div class="photo-banner"></div>
  <div class="overlap-card">A card pulled up by 40px to overlap the banner.</div>
</body>
</html>
```

Open it. Read each demo in order.

## Common mistakes

- **Using margin to push a child into the right place inside a parent** — wrong tool. Use Flexbox or Grid alignment instead.
- **`margin: auto` not centering** — you forgot to set a `width` (or used `max-width` instead).
- **Negative margin extends past the parent** — usually OK but if you want it to "stay inside", set `overflow: hidden` or use a slightly different layout.

## Debugging tips

- DevTools → Computed → Box Model. The margin is **orange** in the diagram.

## Quick quiz

1. What does `margin: 0 auto` do?
2. What does `margin-left: auto` do inside a flex row?
3. Are vertical margins collapsed in flex containers?

<details>
<summary>Answers</summary>

1. Centers a block element horizontally (only when the element has a non-auto width).
2. Pushes the element to the right edge, taking all remaining space.
3. No. Flex (and Grid) items don't collapse margins.

</details>

## Mini challenge

Build a "prayer times" widget — a centered card 90% wide, capped at 400px, with a header inside. Add a small badge in the top-right using `margin-left: auto` inside a flex row.

## What's next

Boxes are sized, padded, and spaced. Now we look at how they participate in flow — `display`.

→ [Lesson 19 — Display & Visibility](19-display.md)
