# Project 1 — Profile Card

> The first capstone. A simple, single-person profile card. The goal is to make a polished component using **only** the basics — display, padding, border, background, hover, and box model. If you can't make this one look beautiful, the rest will be hard. So take your time.

## What you'll build

A vertical card centered on the page that contains:

- A circular avatar image (or SVG).
- A name, a role/headline, a one-liner bio.
- Two social links ("Message" + "Follow").
- A pill that says "Online" with a green dot.

It must look great at any screen size — readable, balanced, no overflow.

## Lessons this builds on

- [Lesson 15 — Box model](../lessons/15-box-model.md)
- [Lesson 16 — Width & max-width](../lessons/16-height-width.md)
- [Lesson 17 — Padding](../lessons/17-padding.md)
- [Lesson 18 — Margins](../lessons/18-margins.md)
- [Lesson 19 — Display](../lessons/19-display.md)
- [Lesson 20 — Outline, overflow, object-fit](../lessons/20-outline-overflow.md)
- [Lesson 22 — Z-index (badge trick)](../lessons/22-z-index.md)
- [Lesson 26 — Pseudo-classes (badge dot)](../lessons/26-pseudo-classes.md)
- [Lesson 32 — Buttons & cards](../lessons/32-buttons-cards.md)

## Specs

| Element | Rule |
|---------|------|
| Card | max-width 340px, white background, padding 24px, border-radius 16px, drop-shadow |
| Avatar | 96×96, perfectly round (`border-radius: 50%`), centered horizontally |
| Name | font-size 20px, weight bold |
| Role | small, color #6b7280 |
| Bio | font-size 14px, line-height 1.6, color #444 |
| Buttons row | 2 buttons side by side, full width of card, equal height |
| "Online" pill | in the top-right corner of the card, absolute position |
| Green dot | 8×8 circle inside the pill |

## Step-by-step plan

1. **Skeleton HTML.** Add a `.card` container with avatar, name, role, bio, button row, online pill.
2. **Card frame.** center on page with flex, set the card width/padding/border-radius/box-shadow.
3. **Avatar.** Place a div for the image, square, centered, then `border-radius: 50%`.
4. **Typography.** Name big, role muted, bio small.
5. **Buttons.** A flex row with gap; primary + secondary.
6. **Online pill.** Position absolutely in the top-right of the card with a green dot using `::after`.
7. **Responsive check.** Resize the window — does it stay readable?
8. **Polish.** Add `:hover` lift on the card, focus ring on buttons.

## Starter HTML

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Profile card</title>
  <style>
    /* TODO: your CSS here */
  </style>
</head>
<body>
  <article class="card">
    <div class="online">Online</div>
    <div class="avatar"></div>
    <h1 class="name">Hauwa Bello</h1>
    <p class="role">Frontend Developer</p>
    <p class="bio">CSS enthusiast. Building beautiful, accessible web UIs one rule at a time.</p>
    <div class="row">
      <button class="btn primary">Message</button>
      <button class="btn secondary">Follow</button>
    </div>
  </article>
</body>
</html>
```

## Reference solution (no peeking — try first!)

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Profile card</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }

    :root {
      --primary: #6d28d9;
      --accent:  #f97316;
      --ink:     #1e293b;
      --muted:   #6b7280;
      --green:   #059669;
    }

    body {
      margin: 0;
      min-height: 100vh;
      display: grid; place-items: center;
      background: linear-gradient(135deg, #fef3c7, #ede9fe);
      font-family: system-ui, sans-serif;
      padding: 20px;
    }

    .card {
      position: relative;
      max-width: 340px;
      width: 100%;
      background: white;
      padding: 24px;
      border-radius: 16px;
      box-shadow: 0 10px 30px rgba(0,0,0,0.1);
      text-align: center;
      transition: transform .2s, box-shadow .2s;
    }
    .card:hover {
      transform: translateY(-4px);
      box-shadow: 0 20px 40px rgba(0,0,0,0.14);
    }

    .avatar {
      width: 96px; height: 96px;
      border-radius: 50%;
      background: linear-gradient(135deg, #6d28d9, #f97316);
      margin: 0 auto 16px;
      display: grid; place-items: center;
      color: white;
      font-size: 36px;
      font-weight: 700;
    }

    .name {
      font-size: 20px;
      font-weight: 700;
      margin: 0 0 4px;
      color: var(--ink);
    }
    .role {
      color: var(--muted);
      font-size: 14px;
      margin: 0 0 12px;
    }
    .bio {
      color: #555;
      font-size: 14px;
      line-height: 1.6;
      margin: 0 0 20px;
    }

    .row {
      display: flex;
      gap: 8px;
    }
    .btn {
      flex: 1;
      border: 0;
      border-radius: 10px;
      padding: 10px 14px;
      font-weight: 600;
      cursor: pointer;
      font: inherit;
    }
    .btn.primary {
      background: var(--primary);
      color: white;
    }
    .btn.primary:hover { background: #5b21b6; }
    .btn.secondary {
      background: #f1f5f9;
      color: var(--ink);
    }
    .btn.secondary:hover { background: #e2e8f0; }
    .btn:focus-visible {
      outline: 3px solid var(--accent);
      outline-offset: 2px;
    }

    .online {
      position: absolute;
      top: 16px; right: 16px;
      background: #d1fae5;
      color: #065f46;
      padding: 4px 10px 4px 18px;
      border-radius: 999px;
      font-size: 11px;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 0.04em;
    }
    .online::before {
      content: "";
      position: absolute;
      left: 8px; top: 50%;
      transform: translateY(-50%);
      width: 6px; height: 6px;
      border-radius: 50%;
      background: var(--green);
      box-shadow: 0 0 0 2px #d1fae5;
    }
  </style>
</head>
<body>
  <article class="card">
    <div class="online">Online</div>
    <div class="avatar">HB</div>
    <h1 class="name">Hauwa Bello</h1>
    <p class="role">Frontend Developer</p>
    <p class="bio">CSS enthusiast. Building beautiful, accessible web UIs one rule at a time.</p>
    <div class="row">
      <button class="btn primary">Message</button>
      <button class="btn secondary">Follow</button>
    </div>
  </article>
</body>
</html>
```

## Self-check (mark each when true)

- [ ] Card is centered on the page.
- [ ] Avatar is exactly circular and centered.
- [ ] Card has a visible shadow that grows on hover.
- [ ] Buttons take equal width and align cleanly.
- [ ] "Online" pill sits in the top-right corner.
- [ ] Green dot sits inside the pill with a halo/border.
- [ ] On a 360 px-wide screen, nothing overflows.
- [ ] Tab into the buttons — focus ring is visible.

## Bonus challenges (do at least one)

1. Add a `clip-path` shape to the avatar (hexagon or rounded-square).
2. Make the "Follow" button toggle to a checkmark on click using `:active` and a transform.
3. Add a "stats" row below the bio (e.g., "1.2k followers · 240 projects").
4. Replace the gradient with an actual image using `object-fit: cover`.

## Once you've finished

Move on to the next project — the patterns you practiced here (card + button row + badge) all return in project 2.

→ [Project 2 — Pricing cards](02-pricing-cards.md)
