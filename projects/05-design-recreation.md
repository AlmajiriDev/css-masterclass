# Project 5 — Design Recreation Challenge

> Reverse-engineering is one of the **best** ways to train your CSS eye. Pick a design (a screenshot or any website you like), then rebuild it pixel-perfect using only HTML and CSS. In this project we provide a brief, you provide the markup.

## What you'll build

Pick **one** of the following briefs:

### Brief A — "Music app now-playing screen"

A phone-shaped mockup containing:

- 280×500 device frame with rounded corners and a screen background.
- Album art (square, rounded 16px, top portion).
- Track name (large), artist name (smaller).
- A progress bar with a thumb.
- Play / pause / skip controls.
- Like and shuffle icons.

### Brief B — "Weather widget card"

A 280-px wide card with:

- A blue gradient background with subtle "sun" blur.
- City name + date.
- A giant temperature number (76°F).
- Conditions ("Sunny").
- A 5-day forecast row (5 mini-cards) at the bottom.

### Brief C — "Coffee shop menu card"

A vertical card with:

- A sepia background image (use a CSS gradient).
- A logo / brand name at the top.
- A list of 4 drinks, each with name + price.
- A divider line.
- A "Open now" pill at the bottom.

## Lessons this builds on

Pretty much everything to date. The challenge is putting **a lot** of small CSS skills together.

## How to approach

1. **Look closely at the design** — every visual detail matters.
2. **Identify the components** — what's a card, a button, a list item?
3. **Estimate sizes** — is the headline 32px or 24px? Is the card rounded or square?
4. **Pick a color palette** — usually 2 – 4 colors plus a background.
5. **Build top to bottom** — start with structure, then visuals.
6. **Use placeholder images** — CSS gradients are perfect for "fake album art."
7. **Adjust iteratively** — pixel-tweak after every major section.

## Reference solution (Brief A)

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Music app screen</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body {
      margin: 0;
      min-height: 100vh;
      display: grid; place-items: center;
      background: linear-gradient(135deg, #1e1b3a, #4c1d95);
      font-family: system-ui, sans-serif;
      padding: 32px;
    }

    /* Phone frame */
    .phone {
      width: 280px;
      height: 540px;
      background: #0f172a;
      border-radius: 36px;
      padding: 8px;
      box-shadow: 0 30px 60px rgba(0,0,0,0.4);
    }
    .screen {
      width: 100%; height: 100%;
      background: linear-gradient(180deg, #1e1b3a, #312e81);
      border-radius: 28px;
      color: white;
      padding: 24px 20px;
      display: flex; flex-direction: column;
      gap: 14px;
    }
    .top {
      display: flex; justify-content: space-between; align-items: center;
      font-size: 12px;
    }
    .top .battery { opacity: 0.7; }

    .album {
      width: 100%;
      aspect-ratio: 1/1;
      border-radius: 16px;
      background:
        radial-gradient(circle at 70% 30%, #fb7185 0 80px, transparent 90px),
        linear-gradient(135deg, #fde68a, #f97316);
    }

    .track  { font-size: 22px; font-weight: 800; margin: 0; line-height: 1.1; }
    .artist { color: #c4b5fd; margin: 4px 0 0; font-size: 14px; }

    .progress {
      display: flex; flex-direction: column; gap: 6px;
    }
    .bar { height: 4px; border-radius: 999px; background: rgba(255,255,255,0.2); position: relative; overflow: hidden; }
    .fill {
      position: absolute;
      inset: 0 60% 0 0;
      background: #fbbf24;
      border-radius: 999px;
    }
    .thumb {
      position: absolute;
      right: 40%; top: 50%;
      transform: translate(50%, -50%);
      width: 10px; height: 10px;
      border-radius: 50%;
      background: white;
    }
    .times { display: flex; justify-content: space-between; font-size: 11px; color: #a5b4fc; }

    .controls {
      display: flex; align-items: center; justify-content: space-between;
      margin-top: 8px;
    }
    .controls button {
      background: transparent;
      border: 0;
      color: white;
      cursor: pointer;
      padding: 0;
    }
    .controls .play {
      width: 64px; height: 64px;
      border-radius: 50%;
      background: white;
      color: #1e1b3a;
      display: grid; place-items: center;
      font-size: 24px;
    }
    .controls .small { font-size: 22px; opacity: 0.8; }
    .controls .small:hover, .controls .play:hover { opacity: 1; transform: scale(1.06); transition: .15s; }

    .extras {
      display: flex; justify-content: space-between;
      color: #a5b4fc;
      font-size: 16px;
    }
  </style>
</head>
<body>
  <div class="phone">
    <div class="screen">
      <div class="top">
        <span>9:41</span>
        <span class="battery">●●●●●</span>
      </div>

      <div class="album"></div>

      <div>
        <p class="track">Sunset Drive</p>
        <p class="artist">Bello &amp; Friends</p>
      </div>

      <div class="progress">
        <div class="bar"><div class="fill"></div><div class="thumb"></div></div>
        <div class="times"><span>1:42</span><span>-1:08</span></div>
      </div>

      <div class="controls">
        <button class="small">⤺</button>
        <button class="small">⏮</button>
        <button class="play">▶</button>
        <button class="small">⏭</button>
        <button class="small">⤻</button>
      </div>

      <div class="extras">
        <span>♥</span>
        <span>⇄</span>
      </div>
    </div>
  </div>
</body>
</html>
```

## Self-check

For your chosen brief:

- [ ] All required elements are present.
- [ ] Spacing looks deliberate (not random).
- [ ] Colors match your reference (or close enough).
- [ ] Interactive elements have hover states.
- [ ] The design feels coherent, not pieced-together.

## Bonus

1. Animate the progress bar with `@property` (the fill grows over time).
2. Toggle between "play" and "pause" iconography (you can use `::before`/`::after` content).
3. Add a small floating "heart" animation when you click ♥.

## Up next

Let's string everything together into a real landing page.

→ [Project 6 — Business Landing Page](06-landing-page.md)
