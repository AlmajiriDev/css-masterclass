# Lesson 12 — Borders & Rounded Corners

> **The picture-frame analogy.** A border is the frame around a photo. It can be a thin black line, a thick gold edge, or a wavy color splash. CSS lets you pick the thickness, the style, the color — and even round the corners so the frame doesn't look like a sharp 1990s rectangle.

## What you'll learn

- The `border` shorthand.
- Per-side borders (`border-top`, `border-left`).
- Border styles (`solid`, `dashed`, `dotted`, etc.).
- `border-radius` and how it makes shapes.

## Why this matters

Borders define the shape of an element. Rounded corners turn "box" into "card". Together they're used on almost every modern component — buttons, inputs, images, cards, pills.

## The 3 border properties

```css
.box {
  border-width: 2px;       /* how thick */
  border-style: solid;     /* what line */
  border-color: #6d28d9;   /* what color */
}
```

You can write each separately or together:

```css
.box { border: 2px solid #6d28d9; }
```

Order is **`width style color`**. You can vary the order — the browser is flexible.

### Border styles

`solid`, `dashed`, `dotted`, `double`, `groove`, `ridge`, `inset`, `outset`, `none`, `hidden`.

> **Note:** `groove`, `ridge`, `inset`, `outset` were cool in the 90s. Today's UI mostly uses `solid`, `dashed`, or no border at all.

```css
.dashed  { border: 2px dashed #6d28d9; }
.dotted  { border: 2px dotted #6d28d9; }
.double  { border: 6px double #6d28d9; }
.none    { border: 0; }
```

## Per-side borders

```css
.box {
  border-top: 4px solid orange;
  border-bottom: 2px solid gray;
  border-left: 0;
}
```

Common pattern: a single underline as a section accent.

```css
h2 {
  border-bottom: 3px solid #f97316;
  padding-bottom: 4px;
}
```

## Border-radius — round the corners

```css
.btn {
  border: 1px solid #6d28d9;
  border-radius: 8px;       /* all 4 corners */
}

.circle {
  width: 100px; height: 100px;
  border-radius: 50%;       /* makes it a circle */
}

.egg {
  border-radius: 50% / 60%; /* horizontal radius 50%, vertical 60% */
}
```

You can target individual corners:

```css
.card { border-radius: 16px 0 16px 0; }    /* top-left, top-right, bottom-right, bottom-left */
.tab  { border-radius: 12px 12px 0 0; }     /* only top rounded */
```

The order is clockwise from top-left: TL, TR, BR, BL. Same as `padding` and `margin`.

### Pill shape

```css
.pill {
  padding: 6px 16px;
  border-radius: 9999px;        /* very large number = pill */
  background: #ede9fe;
}
```

A number larger than half the element's height always rounds to a pill.

## Try it yourself

Create `practice/lesson12.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 12 — borders</title>
  <style>
    body {
      font-family: system-ui, sans-serif;
      padding: 30px;
      background: #fffbf5;
      display: grid;
      gap: 16px;
      grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    }

    .box {
      padding: 20px;
      text-align: center;
      background: white;
    }

    .solid   { border: 3px solid #6d28d9; }
    .dashed  { border: 3px dashed #6d28d9; }
    .dotted  { border: 3px dotted #6d28d9; }
    .double  { border: 6px double #6d28d9; }
    .sides   {
      border-top:    4px solid #f97316;
      border-bottom: 2px solid #6d28d9;
      border-left:   0;
      border-right:  0;
    }
    .rounded { border: 2px solid #6d28d9; border-radius: 16px; }
    .pill {
      border: 1px solid #6d28d9;
      border-radius: 9999px;
      padding: 8px 18px;
    }
    .circle {
      width: 120px; height: 120px;
      border: 4px solid #f97316;
      border-radius: 50%;
      display: flex; align-items: center; justify-content: center;
      margin: 0 auto;
    }
    .tab { border: 1px solid #ddd; border-radius: 12px 12px 0 0; }
    .focus-ring:focus {
      outline: none;
      border: 2px solid #6d28d9;
      box-shadow: 0 0 0 3px rgba(109, 40, 217, 0.3);  /* Lesson 13 — shadow */
    }
  </style>
</head>
<body>
  <div class="box solid">solid</div>
  <div class="box dashed">dashed</div>
  <div class="box dotted">dotted</div>
  <div class="box double">double</div>
  <div class="box sides">top + bottom only</div>
  <div class="box rounded">rounded 16px</div>
  <div class="box pill">pill</div>
  <div class="box circle">circle</div>
  <div class="box tab">tab (top only)</div>
  <input class="box focus-ring" placeholder="focus for ring">
</body>
</html>
```

Open it. Click the input — you'll see a "focus ring" using a shadow (preview of Lesson 13).

## Common mistakes

- **Border on inline elements** — vertical borders on `<a>` or `<span>` look wrong. Make them `inline-block` or `block` first.
- **Rounding collapsed through siblings** — corner radius doesn't fix the gap between two stacked boxes.
- **`border: 0;` vs `border: none;`** — both work, but `0` is shorter.

## Debugging tips

- If a border doesn't show, check that you didn't accidentally set `border: 0` or `border-style: none` higher up the cascade.
- For sharp debugging, use `border: 1px dashed red !important;` to confirm the element exists.

## Quick quiz

1. Write a rule that puts a 2px black border only on the bottom.
2. How do you make a square `<div>` into a circle?
3. What's the difference between `border-radius: 8px` and `border-radius: 8px 0`?

<details>
<summary>Answers</summary>

1. `border-bottom: 2px solid black;`
2. `width: 100px; height: 100px; border-radius: 50%;`
3. The first sets all four corners to 8px. The second sets top-left & bottom-right to 8px and top-right & bottom-left to 0.

</details>

## Mini challenge

Build a **profile badge** — a circle holding a letter, with a colored border, sitting inside a pill-shaped label that says "Online". Use just borders and `border-radius`.

## What's next

Borders frame things. Shadows make them float. Gradients make them glow.

→ [Lesson 13 — Shadows & Gradients](13-shadows-gradients.md)
