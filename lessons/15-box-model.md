# Lesson 15 — The Box Model

> **The shipping-box analogy.** Every HTML element is a cardboard box. The **content** is the thing you ship. **Padding** is the bubble wrap inside the box. The **border** is the cardboard wall. **Margin** is the space between this box and the next box on the shelf. Learn this and CSS layout makes sense for life.

## What you'll learn

- The 4 layers of every box: content, padding, border, margin.
- How `width` and `height` actually measure.
- `box-sizing: border-box` (the **most useful** CSS reset).
- Margin collapse — the only weird rule in CSS.

## Why this matters

You will fight CSS until you understand the box model. After this lesson, layout problems become "where's my padding/border/margin wrong?" instead of mystery.

## The 4 layers, outside-in

```
+-----------------------------+
|         margin              |   outside space
|  +-----------------------+  |
|  |       border          |  |   wall
|  |  +-----------------+  |  |
|  |  |     padding     |  |  |   bubble wrap
|  |  | +-------------+ |  |  |
|  |  | |   content   | |  |  |   the actual thing
|  |  | +-------------+ |  |  |
|  |  +-----------------+  |  |
|  +-----------------------+  |
+-----------------------------+
```

The total space the box takes on the page is:

```
total width  = width + padding-left + padding-right + border-left + border-right + margin-left + margin-right
total height = same but vertical
```

## Try it — the simple version

```css
.box {
  width: 200px;
  padding: 20px;
  border: 5px solid #6d28d9;
  margin: 30px;
}
```

Final width = 200 + 40 (padding) + 10 (border) = **250px**.
Final space the box carves out = 250 + 60 (margin) = **310px wide**.

This default is called `box-sizing: content-box`.

## `box-sizing: border-box` — the life-saver

```css
*, *::before, *::after { box-sizing: border-box; }
```

After this reset, `width: 200px` means the **whole box (padding + border included) is 200px**. The content area shrinks to fit. Predictable layout forever.

Always put this reset at the top of every stylesheet you write.

## Margin collapse (the quirk)

Two **vertical** margins collapse into one:

```html
<p>A</p>
<p>B</p>
```

```css
p { margin: 20px; }   /* each margin 20px */
```

The gap between them? **20px**, not 40px. The bigger one wins.

This does NOT happen with horizontal margins, padding, borders, or Flexbox/Grid children.

To stop the collapse, add any of:

- `padding` to the parent
- `border` to the parent
- `overflow: hidden` to the parent
- A non-zero `min-height` to the parent
- A float or flex context

We'll come back to this when it bites you.

## Try it yourself

Create `practice/lesson15.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 15 — box model</title>
  <style>
    /* THE reset every beginner should memorize */
    *, *::before, *::after { box-sizing: border-box; }

    body {
      font-family: system-ui, sans-serif;
      padding: 30px;
      background: #fffbf5;
    }

    .compare {
      display: flex;
      gap: 30px;
      flex-wrap: wrap;
    }

    .box {
      width: 200px;
      height: 100px;
      padding: 20px;
      border: 5px solid #6d28d9;
      background: #ede9fe;
    }

    /* without reset */
    .no-reset   { box-sizing: content-box; }
    /* with reset */
    .with-reset { box-sizing: border-box; }

    .label {
      margin-top: 8px;
      text-align: center;
      font-size: 12px;
      color: #555;
    }
    .note {
      width: 240px;
      padding: 12px;
      border: 2px dashed #6d28d9;
      text-align: center;
    }

    /* margin collapse demo */
    .stack p {
      margin: 20px;
      background: #fef3c7;
    }
    .fixed p {
      margin: 20px;
      background: #fef3c7;
      overflow: hidden;        /* kills margin collapse */
    }
  </style>
</head>
<body>
  <h1>The box model</h1>

  <h2>Compare content-box vs border-box</h2>
  <div class="compare">
    <div>
      <div class="box no-reset">content-box</div>
      <div class="label">width says 200px but box is 250px</div>
    </div>
    <div>
      <div class="box with-reset">border-box</div>
      <div class="label">width says 200px and box is 200px</div>
    </div>
  </div>

  <h2>Margin collapse</h2>
  <div class="stack">
    <p>Margin 20px</p>
    <p>Margin 20px</p>
  </div>
  <p class="note">Gap above is 20px, not 40px. The two margins collapsed.</p>

  <h3>How to stop it</h3>
  <div class="fixed">
    <p>Margin 20px (parent has overflow: hidden)</p>
    <p>Margin 20px</p>
  </div>
  <p class="note">Gap above is 40px now — collapse killed.</p>
</body>
</html>
```

Open it. Inspect the first two boxes side by side. The `content-box` one overflows the 200px width on screen because padding + border are **added**. The `border-box` one stays at exactly 200px.

## Common mistakes

- **Forgetting `box-sizing: border-box`** — and then spending an hour wondering why padding pushed everything wider.
- **Margin collapse surprise** — paragraphs that don't separate as much as you think.
- **Negative margins** work (overlap), but only use them on purpose.

## Debugging tips

- In Chrome DevTools → Computed → look at the "Box Model" tab. It draws the four layers visually for the selected element. Use it every time you're confused.

## Quick quiz

1. Name the 4 layers of the box model from inside to outside.
2. With `box-sizing: border-box; width: 100px; padding: 10px; border: 2px solid black;`, what's the visible box width?
3. Two siblings with `margin-top: 20px` and `margin-bottom: 30px` — what's the gap between them?

<details>
<summary>Answers</summary>

1. Content → Padding → Border → Margin.
2. 100px (everything fits inside).
3. 30px (the larger of the two collapses with the other).

</details>

## Mini challenge

Make a 3-column "summary cards" section where each card is exactly `width: 30%` and they still have **20px** gaps. Bonus: no horizontal scrolling.

## What's next

Now we measure and resize — height, width, max-width.

→ [Lesson 16 — Height, Width & Max-width](16-height-width.md)
