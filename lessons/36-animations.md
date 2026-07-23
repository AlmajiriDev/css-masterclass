# Lesson 36 — Animations

> **The dancer analogy.** Transitions let an element move from one pose to another. Animations let an element **dance** — a chain of poses following a beat, looping forever or once. Where transitions are "from A to B", animations are a whole choreography.

## What you'll learn

- The `@keyframes` rule.
- The `animation` shorthand.
- Animation properties: name, duration, timing, delay, iteration, direction, fill-mode, play-state.
- Practical animations: spinners, pulses, slides.

## Why this matters

Some things in real life are not "states" — they happen continuously. Spinners, bouncing loaders, attention-grabbing pulses, parallax-on-scroll. CSS animations are the answer.

## The 4 ingredients

```css
@keyframes name {
  from { /* start state */ }
  to   { /* end state */ }
}

.box {
  animation: name 2s ease-in-out infinite;
}
```

You can also use percentages for multi-step keyframes:

```css
@keyframes pulse {
  0%, 100% { transform: scale(1); opacity: 1; }
  50%      { transform: scale(1.05); opacity: 0.8; }
}

.box {
  animation: pulse 2s ease-in-out infinite;
}
```

## The animation shorthand

```css
animation: <name> <duration> <timing-function> <delay> <iteration-count> <direction> <fill-mode> <play-state>;
```

Or spelled out:

```css
animation-name: pulse;
animation-duration: 2s;
animation-timing-function: ease-in-out;
animation-delay: 0s;
animation-iteration-count: infinite;
animation-direction: normal;
animation-fill-mode: none;
animation-play-state: running;
```

## Iteration & direction

```css
animation-iteration-count: infinite;
animation-iteration-count: 3;
animation-direction: normal;       /* forward */
animation-direction: reverse;      /* backward */
animation-direction: alternate;    /* forward, then backward, repeating */
animation-direction: alternate-reverse;
```

`alternate` is your friend for "bounce" effects.

## Fill mode — what the element looks like before and after

```css
animation-fill-mode: none;        /* default — element snaps to base state */
animation-fill-mode: forwards;     /* keep last keyframe after done */
animation-fill-mode: backwards;    /* apply first keyframe BEFORE start (during delay) */
animation-fill-mode: both;        /* both */
```

`forwards` is essential for "play once, stay in the final state."

## Practical recipes

### Spinner

```css
@keyframes spin {
  to { transform: rotate(360deg); }
}
.spinner {
  width: 32px; height: 32px;
  border: 4px solid #ede9fe;
  border-top-color: #6d28d9;
  border-radius: 50%;
  animation: spin 0.8s linear infinite;
}
```

### Pulse

```css
@keyframes pulse {
  0%, 100% { transform: scale(1); }
  50%      { transform: scale(1.08); }
}
.pulse { animation: pulse 1.5s ease-in-out infinite; }
```

### Slide in

```css
@keyframes slideInLeft {
  from { transform: translateX(-100%); opacity: 0; }
  to   { transform: translateX(0); opacity: 1; }
}
.slide { animation: slideInLeft .5s ease-out both; }
```

`both` applies the start state during any delay and keeps the end state after.

### Bounce

```css
@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  25%      { transform: translateY(-10px); }
  50%      { transform: translateY(0); }
  75%      { transform: translateY(-5px); }
}
.bouncer { animation: bounce 1s ease-in-out infinite; }
```

### Attention grab

```css
@keyframes shake {
  0%, 100% { transform: translateX(0); }
  20%, 60% { transform: translateX(-6px); }
  40%, 80% { transform: translateX(6px); }
}
.shake:hover { animation: shake .5s; }
```

## Multi-element orchestration

Stagger child animations with `animation-delay`:

```css
.list > * {
  animation: slideInLeft .4s ease-out both;
}
.list > *:nth-child(1) { animation-delay: .0s; }
.list > *:nth-child(2) { animation-delay: .1s; }
.list > *:nth-child(3) { animation-delay: .2s; }
.list > *:nth-child(4) { animation-delay: .3s; }
```

## Accessibility

Same rule as transitions: respect reduced motion.

```css
@media (prefers-reduced-motion: reduce) {
  * { animation-duration: 0.01ms !important; animation-iteration-count: 1 !important; }
}
```

## Try it yourself

Create `practice/lesson36.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 36 — animations</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body { font-family: system-ui, sans-serif; padding: 30px; background: #fffbf5; }

    @media (prefers-reduced-motion: reduce) {
      * { animation-duration: 0.01ms !important; animation-iteration-count: 1 !important; }
    }

    h2 { color: #1e293b; margin-top: 30px; }

    .stage { display: flex; gap: 24px; align-items: center; flex-wrap: wrap; }

    /* spinner */
    .spinner {
      width: 40px; height: 40px;
      border: 5px solid #ede9fe;
      border-top-color: #6d28d9;
      border-radius: 50%;
      animation: spin 0.8s linear infinite;
    }
    @keyframes spin { to { transform: rotate(360deg); } }

    /* pulse dot */
    .pulse {
      width: 40px; height: 40px;
      background: #f97316;
      border-radius: 50%;
      animation: pulse 1.5s ease-in-out infinite;
    }
    @keyframes pulse {
      0%, 100% { transform: scale(1); }
      50%      { transform: scale(1.4); }
    }

    /* slide-in list */
    .slide-list { list-style: none; margin: 0; padding: 0; }
    .slide-list li {
      background: white; padding: 12px 16px; margin-bottom: 8px;
      border-radius: 8px; box-shadow: 0 2px 6px rgba(0,0,0,0.06);
      animation: slideInLeft .4s ease-out both;
    }
    .slide-list li:nth-child(1) { animation-delay: .0s; }
    .slide-list li:nth-child(2) { animation-delay: .1s; }
    .slide-list li:nth-child(3) { animation-delay: .2s; }
    .slide-list li:nth-child(4) { animation-delay: .3s; }
    @keyframes slideInLeft {
      from { transform: translateX(-20px); opacity: 0; }
      to   { transform: translateX(0); opacity: 1; }
    }

    /* bouncer */
    .bouncer {
      width: 60px; height: 60px;
      background: #10b981;
      border-radius: 50%;
      animation: bounce 1s ease-in-out infinite;
    }
    @keyframes bounce {
      0%, 100% { transform: translateY(0); }
      50%      { transform: translateY(-30px); }
    }

    /* shake on hover */
    .shake {
      background: #6d28d9; color: white;
      padding: 10px 18px; border: 0; border-radius: 8px;
      cursor: pointer; font: inherit;
    }
    .shake:hover { animation: shake .5s; }
    @keyframes shake {
      0%, 100% { transform: translateX(0); }
      25%      { transform: translateX(-8px); }
      75%      { transform: translateX(8px); }
    }

    /* progress bar */
    .bar {
      width: 100%; height: 10px;
      background: #ede9fe;
      border-radius: 999px;
      overflow: hidden;
      max-width: 600px;
    }
    .bar::after {
      content: "";
      display: block;
      width: 30%; height: 100%;
      background: linear-gradient(90deg, #6d28d9, #f97316);
      animation: load 2s ease-in-out infinite alternate;
    }
    @keyframes load {
      from { width: 10%; }
      to   { width: 100%; }
    }
  </style>
</head>
<body>
  <h1>CSS animations</h1>

  <h2>Spinner</h2>
  <div class="stage"><div class="spinner"></div>Loading…</div>

  <h2>Pulse</h2>
  <div class="stage"><div class="pulse"></div>Notification dot</div>

  <h2>Slide-in list (staggered)</h2>
  <ul class="slide-list">
    <li>First item</li>
    <li>Second item</li>
    <li>Third item</li>
    <li>Fourth item</li>
  </ul>

  <h2>Bouncing dot</h2>
  <div class="stage"><div class="bouncer"></div></div>

  <h2>Shake on hover</h2>
  <button class="shake">Hover me</button>

  <h2>Progress bar</h2>
  <div class="bar"></div>
</body>
</html>
```

## Common mistakes

- **Animating `display` or `width: auto`** — doesn't work. Use transforms or fixed values.
- **Long infinite animations** — battery drain and motion sickness.
- **Animating layout-affecting properties** (`width`, `height`, `top`, `left`) — uses the slow layout pipeline. Use `transform` and `opacity` instead (they're GPU-accelerated).

## Performance tips

- Animate `transform` and `opacity` for buttery-smooth 60fps.
- Avoid animating `width`, `height`, `margin`, `top`, `left`.
- Use `will-change: transform` if you'll be animating the same element repeatedly, but reset it after.

## Debugging tips

- In DevTools → Animations tab, pause animations and scrub.
- The `prefers-reduced-motion` toggle in DevTools (Rendering tab) lets you test.

## Quick quiz

1. What's the difference between `animation-iteration-count: 3` and `infinite`?
2. What does `animation-fill-mode: forwards` do?
3. Which two properties should you animate for best performance?

<details>
<summary>Answers</summary>

1. `3` plays 3 times and stops. `infinite` plays forever.
2. Keeps the final keyframe's state after the animation ends.
3. `transform` and `opacity`.

</details>

## Mini challenge

Build a "notification toast" that slides in from the bottom-right, stays for 3 seconds, then fades out — all using only CSS animations. Use `forwards` for the final state.

## What's next

We can move things. Now one more step — sprites, masks, and other advanced visuals.

→ [Lesson 37 — Sprites, Masks & Text Effects](37-text-effects-sprites.md)
