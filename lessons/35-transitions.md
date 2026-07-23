# Lesson 35 — Transitions

> **The spring analogy.** A transition tells a property: "go from your current state to the new state **smoothly** over time, like a spring." Without it, hovers and toggles feel like a light switch — on, off, instant. With it, they feel like a soft button.

## What you'll learn

- The 4 transition properties (`transition-property`, `duration`, `timing-function`, `delay`).
- Common easing functions.
- How to animate multiple properties at once.
- `prefers-reduced-motion` — the accessibility gotcha.

## Why this matters

Transitions are how you tell CSS "yes, I want this to feel alive." Used well, they make everything feel intentional.

## The transition shorthand

```css
.btn {
  background: white;
  transition: background 0.2s ease;
}
.btn:hover {
  background: #6d28d9;
}
```

Format:

```
transition: <property> <duration> <timing-function> <delay>
```

Example:

```css
transition: opacity 0.3s ease-in-out 0.1s;
```

## The 4 long-form properties

```css
transition-property:        background, transform, opacity;
transition-duration:        0.2s, 0.4s, 0.2s;
transition-timing-function: ease, ease-out, ease-in-out;
transition-delay:           0s, 0s, 0s;
```

## Timing functions

| Function | Curve | Use for |
|----------|-------|---------|
| `linear` | straight line | progress bars |
| `ease` | smooth slow | default |
| `ease-in` | starts slow, ends fast | element leaving |
| `ease-out` | starts fast, ends slow | element entering |
| `ease-in-out` | slow at both ends | smooth interactive |
| `cubic-bezier(x1, y1, x2, y2)` | custom | designer control |

`cubic-bezier` lets you choreograph the motion precisely. Sites like [easings.net](https://easings.net) preview curves for you.

## Common patterns

### Button hover

```css
.btn {
  background: #6d28d9;
  color: white;
  padding: 10px 18px;
  border: 0;
  border-radius: 8px;
  cursor: pointer;
  transition: background .2s ease, transform .1s ease, box-shadow .2s ease;
}
.btn:hover {
  background: #5b21b6;
  transform: translateY(-1px);
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
}
.btn:active {
  transform: translateY(1px);
}
```

### Opacity fade

```css
.toast {
  opacity: 0;
  transform: translateY(8px);
  transition: opacity .3s ease, transform .3s ease;
}
.toast.show {
  opacity: 1;
  transform: translateY(0);
}
```

### Smooth size change

```css
.card {
  width: 200px;
  transition: width .3s ease, padding .3s ease;
}
.card:hover {
  width: 240px;
}
```

## `transition: all` — convenient but dangerous

```css
.bad    { transition: all .3s ease; }     /* catches every property */
.better { transition: transform .3s, opacity .3s, background .3s; }   /* explicit */
```

`all` is **slow** in real apps because the browser doesn't know what to optimize. Prefer explicit properties.

## Accessibility — `prefers-reduced-motion`

Some users have vestibular disorders and get sick from motion. Always respect their preference:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

Or, less aggressive:

```css
.card { transition: transform .3s ease; }

@media (prefers-reduced-motion: reduce) {
  .card { transition: none; }
}
```

Add **at least one** of these to your project. It's a small courtesy.

## Transitions in DevTools

- In Chrome DevTools → Animations tab, you can pause and scrub CSS transitions to debug.

## Try it yourself

Create `practice/lesson35.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 35 — transitions</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body { font-family: system-ui, sans-serif; padding: 30px; background: #fffbf5; }

    /* Respect reduced motion */
    @media (prefers-reduced-motion: reduce) {
      * { animation-duration: 0.01ms !important;
          transition-duration: 0.01ms !important; }
    }

    .row { display: flex; gap: 12px; flex-wrap: wrap; align-items: center; margin-bottom: 30px; }

    /* button */
    .btn {
      padding: 12px 24px;
      border: 0;
      border-radius: 8px;
      background: #6d28d9;
      color: white;
      font-weight: 600;
      cursor: pointer;
      transition: background .2s, transform .15s, box-shadow .2s;
    }
    .btn:hover { background: #5b21b6; transform: translateY(-2px); box-shadow: 0 6px 14px rgba(0,0,0,0.15); }
    .btn:active { transform: translateY(1px); }

    /* different timing functions side by side */
    .timing-btn {
      padding: 10px 18px; border: 0; background: #f97316; color: white;
      border-radius: 6px; cursor: pointer; transition: transform .4s;
    }
    .linear   { transition-timing-function: linear; }
    .ease-in  { transition-timing-function: ease-in; }
    .ease-out { transition-timing-function: ease-out; }
    .bounce   { transition-timing-function: cubic-bezier(.68,-0.55,.27,1.55); }

    .timing-btn:hover { transform: translateX(40px); }

    /* fade in box */
    .fade {
      width: 200px; height: 100px;
      background: #ede9fe;
      border-radius: 12px;
      display: grid; place-items: center;
      opacity: 0;
      transform: translateY(10px);
      transition: opacity .5s ease, transform .5s ease;
    }
    .fade.show { opacity: 1; transform: translateY(0); }

    /* morphing card */
    .morph {
      width: 120px; height: 120px;
      background: #059669;
      border-radius: 12px;
      transition: width .4s ease, height .4s ease, background .4s ease;
    }
    .morph:hover {
      width: 200px;
      height: 160px;
      background: #f59e0b;
    }

    button { font: inherit; }
  </style>
</head>
<body>
  <h1>Transitions</h1>

  <h2>Button hover + press</h2>
  <div class="row">
    <button class="btn">Hover &amp; press</button>
  </div>

  <h2>Different timing functions</h2>
  <div class="row">
    <button class="timing-btn linear">linear</button>
    <button class="timing-btn ease-in">ease-in</button>
    <button class="timing-btn ease-out">ease-out</button>
    <button class="timing-btn bounce">bounce</button>
  </div>

  <h2>Fade-in (toggle by clicking)</h2>
  <button class="btn" onclick="document.querySelector('.fade').classList.toggle('show')">Toggle fade</button>
  <div class="fade" style="margin-top: 12px">I fade in</div>

  <h2>Morphing box</h2>
  <div class="morph"></div>
</body>
</html>
```

## Common mistakes

- **`transition: all`** — slow. Use explicit property names.
- **Transitioning `display`** — doesn't animate. Use opacity + `pointer-events: none` instead.
- **Long durations (>0.5s)** — feels slow. Keep hovers around 0.15-0.3s.

## Quick quiz

1. Write a rule that fades an element over 0.5s with a 0.2s delay.
2. What's wrong with `transition: all .3s`?
3. What media query respects users who don't want motion?

<details>
<summary>Answers</summary>

1. `transition: opacity .5s ease .2s;`
2. It's a perf hit — browser can't optimize what it doesn't know is changing.
3. `@media (prefers-reduced-motion: reduce)`.

</details>

## Mini challenge

Build a navigation link that animates an underline sliding into place from left to right on hover. Use `::after` with `transform: scaleX(0)` to `scaleX(1)` and `transform-origin: left`.

## What's next

Transitions handle changes between two states. Now we animate **continuously** with `@keyframes`.

→ [Lesson 36 — Animations](36-animations.md)
