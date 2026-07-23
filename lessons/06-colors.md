# Lesson 06 — Colors in CSS

> **The paint-mixing analogy.** CSS gives you three different ways to mix paint: **named colors** (the labeled bottles), **hex codes** (precise recipes like a chef's), and **rgb/hsl** (live mixing with dials). All three produce the same paint in the end. Pick whatever feels easiest for the moment.

## What you'll learn

- The 5 ways to write a color in CSS.
- Transparency with `alpha`.
- Naming conventions (semantic vs literal).
- How to read a hex code like a pro.

## Why this matters

Color is 50% of a design's personality. Two sentences can have the same meaning, but the right color makes them feel warm, urgent, luxurious, or playful.

## The 5 ways to write a color

### 1. Named colors

```css
color: tomato;
background: dodgerblue;
border-color: rebeccapurple;
```

There are about 140 named colors. The full list is in `CHEATSHEET.md`'s reference section.

Pros: readable. Cons: limited — you can't ask for "slightly darker blue" precisely.

### 2. Hex (`#rrggbb`)

```css
color: #ff6347;   /* same as 'tomato' */
color: #6d28d9;   /* a deep purple */
color: #f00;      /* 3-digit shorthand = #ff0000 */
```

Reading a hex code:

```
#  R R G G B B
   FF 63 47     → tomato
   6D 28 D9     → purple
   F  F  0      → pure red
```

- `00` = none of that channel.
- `FF` = full of that channel.
- `#rgb` is shorthand when each pair has matching digits.

### 3. `rgb()` and `rgba()`

```css
color: rgb(255, 99, 71);
color: rgba(255, 99, 71, 0.5);   /* 0.5 = 50% see-through */
```

Same colors, different syntax. `rgba`'s last value is **alpha** — how transparent it is. 0 is invisible, 1 is fully solid.

Modern CSS also allows `rgb(255 99 71)` (no commas) and `rgb(255 99 71 / 50%)`.

### 4. `hsl()` and `hsla()` — the designer's favorite

```css
color: hsl(9, 100%, 64%);     /* same tomato */
color: hsla(9, 100%, 64%, 0.5);
```

`hsl()` means:
- `H` = hue (the actual color, 0 – 360)
- `S` = saturation (how vivid, 0% – 100%)
- `L` = lightness (dark to light, 0% – 100%)

Why designers love it: tweaking **L** by 10 points makes it dark or light, no hex math needed.

### 5. `color()` — wider gamut (modern displays)

```css
color: color(display-p3 1 0.466 0.659);
```

You can ignore this for now. It exists for ultra-wide color displays (some Apple devices, OLED TVs).

## Transparency — `alpha`

```css
background: rgba(109, 40, 217, 0.1);   /* very light purple */
```

Or, prefer the modern syntax:

```css
background: rgb(109 40 217 / 10%);
```

Or use `transparent` for fully see-through:

```css
border: 1px solid transparent;
```

`transparent` and `rgba(0,0,0,0)` are the same.

## Currentcolor — the "use mine" keyword

```css
button {
  color: tomato;
  border: 2px solid currentColor;   /* "use whatever my color is" */
}
```

`currentColor` is a magic word. It takes the value of `color`. Great for matching borders, outlines, shadows and SVGs.

## Naming your colors

Don't write `color: #ff4500` for a "primary button" — write:

```css
:root {
  --primary: #6d28d9;
  --accent:  #f97316;
  --ink:     #1e293b;
}
.btn { background: var(--primary); }
```

(Variables will get their own lesson — Lesson 47. For now, the takeaway is: **name colors by purpose**, not by appearance.)

## Try it yourself

Create `practice/lesson06.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 06</title>
  <style>
    body {
      font-family: system-ui, sans-serif;
      padding: 30px;
      background: #fffbf5;
    }

    .row { display: flex; gap: 12px; flex-wrap: wrap; }

    .swatch {
      width: 120px;
      height: 120px;
      border-radius: 12px;
      color: white;
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: 600;
      text-align: center;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    }

    /* named */
    .s1 { background: tomato; }
    /* hex */
    .s2 { background: #6d28d9; }
    /* hex shorthand */
    .s3 { background: #f0f; }      /* same as #ff00ff */
    /* rgb */
    .s4 { background: rgb(34, 197, 94); }
    /* rgba */
    .s5 { background: rgba(34, 197, 94, 0.5); }
    /* hsl */
    .s6 { background: hsl(220, 90%, 56%); }
    /* currentcolor trick */
    .s7 {
      background: transparent;
      color: #6d28d9;
      border: 4px solid currentColor;
    }
  </style>
</head>
<body>
  <h1>Color swatches</h1>
  <div class="row">
    <div class="swatch s1">tomato</div>
    <div class="swatch s2">#6d28d9</div>
    <div class="swatch s3">#f0f</div>
    <div class="swatch s4">rgb()</div>
    <div class="swatch s5">rgba() 0.5</div>
    <div class="swatch s6">hsl()</div>
    <div class="swatch s7">currentcolor</div>
  </div>
</body>
</html>
```

Open it. Look at each swatch. Some are pure color, one is transparent and the text shows through.

## Common mistakes

- **Forgot `#`** before a hex — `color: 6d28d9;` is invalid, browser throws it out.
- **Wrong number of digits** — `#6d28d9` is 6, `#6d2` is 3. Mixing is not allowed.
- **Did you mean opacity or alpha?** `opacity: 0.5` makes the whole element half-transparent (including text). `rgba()` makes only the color half-transparent. Big difference.
- **Hex letters** — `#6d28d9` is lowercase, `#6D28D9` is the same. CSS doesn't care.

## Debugging tips

- In Chrome DevTools, click the small color square next to any color rule — a full color picker appears. You can sample colors from the page itself with the eye-dropper.
- Use [coolors.co](https://coolors.co) or [realtimecolors.com](https://www.realtimecolors.com) to build palettes.

## Quick quiz

1. Write the same color in `named`, `hex`, `rgb`, and `hsl` syntax for *deep sky blue*.
2. What's the difference between `opacity: 0.5` and `rgba(0,0,0,0.5)`?
3. What does `currentColor` mean?

<details>
<summary>Answers</summary>

1. Named: `deepskyblue`. Hex: `#00bfff`. RGB: `rgb(0,191,255)`. HSL: `hsl(195,100%,50%)`.
2. `opacity` affects the **entire element** (text, border, background). `rgba` only affects the color you apply it to.
3. "Use whatever value `color` has."

</details>

## Mini challenge

Build a 5-color **palette swatch** (5 squares side by side) with hex codes, then below the swatches show the **hex codes** as text in each square's color. Hint: use `color: var(--brand1)` etc.

## What's next

We can paint. Now let's decide **how big** — pixels, ems, rems, percent, viewport… the world of CSS units.

→ [Lesson 07 — Units & Values](07-units.md)
