# Lesson 34 — Transforms (2D & 3D)

> **The puppet-string analogy.** Transforms let you **move**, **spin**, **stretch**, or **shrink** an element without affecting the layout around it. They're like grabbing the corners of a square of paper and tilting it in space — the page underneath doesn't change.

## What you'll learn

- The 6 basic transforms: translate, scale, rotate, skew.
- The transform-origin property.
- 3D transforms and perspective.
- When to combine transforms in animations.

## Why this matters

Every hover-lift button, every rotated card, every "tilt your head to see" parallax uses transforms. Combined with transitions and animations (next lessons), they make a UI feel alive.

## The 2D `transform` functions

### translate (move)

```css
.box { transform: translate(10px, -20px); }   /* x, y */
```

Positive `x` goes right, positive `y` goes down. Use any unit, including `%` (relative to **the element itself**!):

```css
.box { transform: translate(-50%, -50%); }   /* centering trick */
```

### scale (resize)

```css
.box { transform: scale(1.05); }   /* 1 = original, >1 bigger, <1 smaller */
.box { transform: scale(2, 1); }   /* x scale, y scale */
```

### rotate (spin)

```css
.box { transform: rotate(45deg); }
```

Positive = clockwise. Negative = counter-clockwise. `turn` is also a unit: `rotate(0.25turn)` = 90°.

### skew (slant)

```css
.box { transform: skew(15deg, 0); }   /* skew x axis */
```

Skew distorts the shape. Use sparingly — it can make text unreadable.

### Combining

```css
.btn { transform: translateY(-2px) scale(1.02); }
```

Order matters: transforms apply **right-to-left** logically. `translate scale` is different from `scale translate`.

## `transform-origin`

By default, transforms rotate/scale around the center. You can change that:

```css
.card {
  transform-origin: top left;     /* rotate from the corner */
  transform: rotate(-10deg);
}
.door {
  transform-origin: left center;  /* hinge on the left */
  transform: rotate(35deg);
}
```

Accepts: keywords (`center`, `top`, `left`, `right`, `bottom`, `top left`…), `%`, length values.

### Useful patterns

```css
/* grow from a corner (menu open) */
.menu-item { transform-origin: top right; }
.menu-item.open { transform: scale(1); }

/* hinge rotation */
.door { transform-origin: left center; }
.door.open { transform: perspective(800px) rotateY(-30deg); }
```

## 3D transforms

For 3D you need two things: a perspective on a parent and 3D functions on children.

```css
.scene {
  perspective: 1000px;          /* how "deep" the 3D feels */
}
.card {
  transform-style: preserve-3d;
  transform: rotateY(40deg);
}
```

### 3D functions

```css
transform: rotateX(20deg);       /* tilt forward/backward */
transform: rotateY(40deg);       /* tilt left/right */
transform: rotateZ(45deg);       /* spin like 2D */
transform: translateZ(50px);     /* push back toward viewer (smaller with perspective) */
transform: translate3d(0, 0, 50px);
```

### A flipping card

```html
<div class="scene">
  <div class="card">
    <div class="face front">Front</div>
    <div class="face back">Back</div>
  </div>
</div>
```

```css
.scene { perspective: 1000px; }
.card  { position: relative; width: 200px; height: 280px; transform-style: preserve-3d; transition: transform .6s; }
.card:hover { transform: rotateY(180deg); }
.face  { position: absolute; inset: 0; display: grid; place-items: center; backface-visibility: hidden; border-radius: 12px; }
.front { background: #6d28d9; color: white; }
.back  { background: #f97316; color: white; transform: rotateY(180deg); }
```

Hover it — the card flips.

### Quick check on perspective

- `perspective` on the **parent** (smaller numbers = more extreme 3D).
- `transform-style: preserve-3d` on the **child** that has the 3D children.
- `backface-visibility: hidden` hides the back face when it rotates around.

## Common uses

```css
/* button press */
button:active { transform: translateY(1px); }

/* card lift */
.card:hover { transform: translateY(-4px); }

/* image zoom */
.zoomable { transition: transform .3s; }
.zoomable:hover { transform: scale(1.1); }

/* parallax layers */
.bg { transform: translateZ(0) scale(1.1); }
```

## Try it yourself

Create `practice/lesson34.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 34 — transforms</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body { font-family: system-ui, sans-serif; padding: 30px; background: #fffbf5; }
    h2 { color: #1e293b; }

    .row {
      display: grid;
      gap: 16px;
      grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
    }
    .box {
      aspect-ratio: 1/1;
      display: grid; place-items: center;
      color: white;
      font-weight: 700;
      border-radius: 12px;
      font-size: 14px;
    }

    /* 2D transforms */
    .translate { background: #6d28d9; transform: translate(10px, -10px); }
    .scale     { background: #f97316; transform: scale(1.05); }
    .rotate    { background: #059669; transform: rotate(15deg); }
    .skew      { background: #0ea5e9; transform: skew(-15deg, 0); padding: 20px; }
    .skew > *  { transform: skew(15deg, 0); }
    .combo     { background: #db2777; transform: translate(-10px, 10px) rotate(20deg); }

    /* transform-origin */
    .from-corner { background: #6366f1; transform-origin: top left; transform: rotate(-15deg); }

    /* 3D */
    .scene3d {
      perspective: 800px;
    }
    .scene3d .flip {
      width: 100%; height: 220px;
      position: relative;
      transform-style: preserve-3d;
      transition: transform .6s;
      border-radius: 12px;
    }
    .scene3d .flip:hover { transform: rotateY(180deg); }
    .scene3d .face {
      position: absolute; inset: 0;
      display: grid; place-items: center;
      color: white; font-size: 18px;
      border-radius: 12px;
      backface-visibility: hidden;
    }
    .scene3d .f1 { background: #6d28d9; }
    .scene3d .f2 { background: #f97316; transform: rotateY(180deg); }
  </style>
</head>
<body>
  <h1>2D &amp; 3D Transforms</h1>

  <h2>2D transforms</h2>
  <div class="row">
    <div class="box translate">translate</div>
    <div class="box scale">scale</div>
    <div class="box rotate">rotate</div>
    <div class="box skew"><span>skew</span></div>
    <div class="box combo">combo</div>
    <div class="box from-corner">from corner</div>
  </div>

  <h2>3D flip card</h2>
  <div class="row scene3d">
    <div class="flip">
      <div class="face f1">Front</div>
      <div class="face f2">Back</div>
    </div>
  </div>
  <p style="color:#6b7280">Hover the card to flip it.</p>
</body>
</html>
```

## Common mistakes

- **Transforms with percentages** — `%` is relative to the **element's own size**, not the parent.
- **Layout doesn't reflow around transforms** — they're paint-only. If you need to push other elements, use `margin` or Flexbox.
- **3D feels flat** — usually missing `perspective` on the parent or `transform-style: preserve-3d` on the child.
- **Rotation of text** — unreadable. Animate only decorative rotation.

## Debugging tips

- In Chrome DevTools, the Animations panel shows transform changes over time. Great for debugging.
- Use the 3D view button (top-right of Elements panel) to see elements in 3D.

## Quick quiz

1. What's the default `transform-origin`?
2. Where does `transform: translate(50%, 0)` move an element 50% relative to?
3. What property on a parent enables 3D for its children?

<details>
<summary>Answers</summary>

1. `center` (50% 50%).
2. The element's own width.
3. `perspective`.

</details>

## Mini challenge

Make a "magnify" effect on a card: hover the card to scale it to 1.1× from the center, and have a small button inside that, on hover, rotates 180°.

## What's next

Transforms are instant. Now we add **motion** — transitions.

→ [Lesson 35 — Transitions](35-transitions.md)
