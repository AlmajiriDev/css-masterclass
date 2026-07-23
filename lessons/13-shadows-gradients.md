# Lesson 13 — Shadows & Gradients

> **The flashlight analogy.** A shadow is the dark shape on the opposite side of an object when light shines. A gradient is when light fades from one color to another — like sunset on a wall. CSS lets you fake light for fun and depth.

## What you'll learn

- `box-shadow` — drop shadows under boxes.
- `text-shadow` — glow around text.
- Linear and radial gradients.
- Layering shadows for depth.
- `border-image` (fancy borders from images).

## Why this matters

A flat page looks like a 2005 form. A page with carefully placed shadows and gradients feels like 2024 mobile UI. These two techniques are why modern design feels "lifted".

## `box-shadow`

```css
.card {
  box-shadow: 4px 6px 12px rgba(0,0,0,0.15);
}
/*        ↑   ↑   ↑      ↑
       x   y  blur  color */
```

Order:

```
[inset] [offset-x] [offset-y] [blur-radius] [spread-radius] [color]
```

- `offset-x`, `offset-y` — where the shadow sits. Positive moves right/down.
- `blur-radius` — bigger = softer. 0 = sharp edge.
- `spread-radius` — grows the shadow outward (or shrinks with negative).
- `color` — usually `rgba` so you can control transparency.
- `inset` — flips the shadow to the inside of the element.

### Multiple shadows

```css
.btn {
  background: white;
  box-shadow:
    0 1px 2px rgba(0,0,0,0.07),
    0 4px 12px rgba(0,0,0,0.1);
}
```

The first shadow makes a subtle line. The second lifts the card.

### Hover lift

```css
.card {
  transition: box-shadow .2s, transform .2s;
}
.card:hover {
  transform: translateY(-4px);
  box-shadow: 0 12px 24px rgba(0,0,0,0.18);
}
```

(We'll cover `transition` deeply in Lesson 35.)

## `text-shadow`

```css
h1 {
  color: white;
  text-shadow: 0 2px 6px rgba(0,0,0,0.5);
}
.glow {
  text-shadow: 0 0 10px #f97316, 0 0 20px #f97316;
}
```

Like `box-shadow` but for text. Works great for hero text overlaid on photos.

## Gradients

### Linear — straight fade

```css
.bg {
  background: linear-gradient(to right, #6d28d9, #f97316);
}
```

Direction can be:

- a side: `to right`, `to left`, `to bottom`, `to top`
- a corner: `to bottom right`, `to top left`
- an angle: `45deg`, `135deg`

### With multiple colors

```css
.sunset {
  background: linear-gradient(to bottom,
    #fde68a 0%,
    #f97316 50%,
    #b45309 100%
  );
}
```

The percentages are **color stops**.

### Hard color stops (no fade)

```css
.stripes {
  background: linear-gradient(90deg,
    #6d28d9 0% 50%,     /* color from 0% to 50% */
    #f97316 50% 100%    /* second color from 50% to 100% */
  );
}
```

This produces a sharp split between purple and orange.

### Radial — round fade

```css
.spot {
  background: radial-gradient(circle at center, #fde68a, transparent);
}
.sun {
  background:
    radial-gradient(circle at 80% 20%, #fde68a 0 100px, transparent 200px),
    linear-gradient(to bottom, #1e3a8a, #60a5fa);
}
```

`radial-gradient(shape at position, color, transparent)`. The shape can be `circle` or `ellipse`.

### Conic gradient — pie slices!

```css
.pie {
  background: conic-gradient(#6d28d9, #f97316, #6d28d9);
  border-radius: 50%;
}
```

Conic gradients sweep around a center. Great for charts, spinners, color wheels.

## Try it yourself

Create `practice/lesson13.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 13 — shadows & gradients</title>
  <style>
    body {
      font-family: system-ui, sans-serif;
      padding: 30px;
      background: #fffbf5;
      display: grid;
      gap: 20px;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
    }

    .card {
      padding: 30px 20px;
      background: white;
      border-radius: 12px;
      text-align: center;
    }

    /* card lift on hover */
    .liftable {
      transition: transform .2s, box-shadow .2s;
      cursor: pointer;
    }
    .liftable:hover {
      transform: translateY(-6px);
      box-shadow: 0 16px 32px rgba(0,0,0,0.18);
    }

    /* 3-layer shadow — soft outer */
    .depth {
      box-shadow:
        0 1px 2px rgba(0,0,0,0.06),
        0 4px 10px rgba(0,0,0,0.08),
        0 16px 40px rgba(0,0,0,0.12);
    }

    /* inset shadow — looks pressed into the surface */
    .pressed {
      box-shadow: inset 0 2px 6px rgba(0,0,0,0.15);
      background: #f5f5f5;
    }

    /* text glow */
    .glow {
      background: #1e1b3a;
      color: white;
      font-weight: bold;
      text-shadow: 0 0 12px #f97316;
    }

    /* gradients */
    .sunset { background: linear-gradient(to bottom right, #fde68a, #f97316, #b45309); color: white; text-shadow: 0 2px 4px rgba(0,0,0,0.4); }
    .sky    { background: linear-gradient(to bottom, #1e3a8a, #60a5fa); color: white; text-shadow: 0 2px 4px rgba(0,0,0,0.4); }
    .neon   { background: linear-gradient(135deg, #6d28d9, #db2777, #f59e0b); color: white; text-shadow: 0 2px 4px rgba(0,0,0,0.4); }
    .hard   {
      background: linear-gradient(90deg, #6d28d9 50%, #f97316 50%);
      color: white;
    }
    .radial {
      background: radial-gradient(circle at center, #fde68a, #f97316);
      color: #1e1b3a;
    }
    .pie {
      background: conic-gradient(#6d28d9 0 25%, #f97316 25% 50%, #fde68a 50% 75%, #059669 75% 100%);
      color: white;
    }
  </style>
</head>
<body>
  <div class="card liftable">Hover me! I lift.</div>
  <div class="card depth">Deep shadow</div>
  <div class="card pressed">Pressed in</div>
  <div class="card glow">Text glow</div>
  <div class="card sunset">Sunset</div>
  <div class="card sky">Sky</div>
  <div class="card neon">Neon angle</div>
  <div class="card hard">Hard split</div>
  <div class="card radial">Radial</div>
  <div class="card pie">Conic</div>
</body>
</html>
```

Open it. Hover the first card — it lifts. The "pie" card is a CSS-only pie chart!

## Common mistakes

- **Hard `box-shadow` color** — use `rgba(0,0,0,0.1)` style; never pure black unless it's strong on purpose.
- **Gradients with too many stops** — 2-3 stops is plenty. 7 looks like a fever dream.
- **`box-shadow` on transparent backgrounds** — invisible. If you can't see a shadow, add background color first.

## Debugging tips

- Want to see exactly where shadows fall? In DevTools, the Computed panel shows the live `box-shadow` value.
- Use [shadows.brumm.af](https://shadows.brumm.af) to copy polished Tailwind-style shadows.

## Quick quiz

1. In `box-shadow: 0 4px 8px rgba(0,0,0,0.2);`, what's the x-offset, y-offset, blur, and color?
2. How do you make a linear gradient go from purple to orange, left to right?
3. Which gradient type is best for a CSS-only pie chart?

<details>
<summary>Answers</summary>

1. x=0, y=4px, blur=8px, color=black 20%.
2. `background: linear-gradient(to right, purple, orange);`
3. `conic-gradient()`.

</details>

## Mini challenge

Build a card with:
- white background
- a 3-layer shadow for depth
- a gradient strip (5px tall) at the top, fading from purple to orange

Use only what we learned.

## What's next

Shadows and gradients make cards look like cards. Now let's add link styles, list styles, and icons.

→ [Lesson 14 — Links, Lists & Icons](14-links-lists-icons.md)
