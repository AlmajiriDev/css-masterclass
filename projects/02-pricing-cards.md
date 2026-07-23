# Project 2 — Pricing Cards

> Three cards side-by-side comparing plan tiers. This is the project where you really feel Flexbox click. Equal heights, the middle one highlighted, and everything responsive on phones.

## What you'll build

A 3-card pricing section:

- **Starter** — basic plan, $0.
- **Pro** — recommended plan, $9 (highlighted, larger, accent border).
- **Team** — group plan, $29.

Each card has:

- A tier name.
- A price (with currency + unit).
- A short description.
- A feature list (4 – 5 items) with checkmarks.
- A CTA button.

The middle card should stand out — bigger, accent border, "Recommended" ribbon.

## Lessons this builds on

- [Lesson 32 — Buttons & cards](../lessons/32-buttons-cards.md)
- [Lesson 38 — Flexbox container](../lessons/38-flexbox.md)
- [Lesson 39 — Flex items](../lessons/39-flexbox-items.md)
- [Lesson 44 — Media queries](../lessons/44-media-queries.md)
- [Lesson 45 — clamp / responsive sizing](../lessons/45-math-functions.md)
- [Lesson 47 — Variables](../lessons/47-variables.md)

## Specs

| Element | Rule |
|---------|------|
| Container | max-width 1100px, centered, padding `clamp(1rem, 4vw, 4rem)` |
| Card row | flex, equal columns (`flex: 1`), aligned at top, gap 16px |
| Default card | background white, padding 24px, border-radius 16px, shadow |
| Pro card | scale 1.05, accent border (orange), pinkish shadow |
| Ribbon | small pill in top-right of pro card |
| Button | full width, primary on Pro, secondary on others |
| Lists | green checkmark before each item (`::before` with content "✓") |
| Responsive | below 800 px, cards stack vertically with a max-width of 480 px each |

## Step-by-step plan

1. **HTML**: 3 cards, each with header, price, description, list, button.
2. **Container**: centered with `width: min(90%, 1100px)` and `margin-inline: auto`.
3. **Row**: flex with gap; cards use `flex: 1`.
4. **Card base**: white, padding, radius, shadow, hover lift.
5. **Pro card**: extra emphasis. Try a transform and accent border.
6. **Ribbon**: pseudo-element or absolute child at top-right.
7. **List styling**: remove bullets, add `::before` with "✓".
8. **Responsive**: at `@media (max-width: 800px)` switch to column layout, width 100% with max.

## Reference solution

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Pricing cards</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }

    :root {
      --primary: #6d28d9;
      --accent:  #f97316;
      --ink:     #1e293b;
      --muted:   #6b7280;
      --green:   #059669;
      --radius:  16px;
    }

    body {
      margin: 0;
      font-family: system-ui, sans-serif;
      background: linear-gradient(180deg, #fffbf5, #f5f3ff);
      padding: clamp(1rem, 4vw, 4rem);
      min-height: 100vh;
    }
    .wrap {
      max-width: 1100px;
      margin: 0 auto;
    }
    .heading {
      text-align: center;
      margin-bottom: clamp(2rem, 4vw, 3rem);
    }
    .heading h1 { color: var(--ink); margin: 0 0 6px; }
    .heading p  { color: var(--muted); margin: 0; }

    .row {
      display: flex;
      gap: 16px;
      align-items: stretch;     /* equal heights */
      flex-wrap: wrap;
    }
    .card {
      flex: 1;
      min-width: 260px;
      background: white;
      padding: 28px 24px;
      border-radius: var(--radius);
      box-shadow: 0 4px 14px rgba(0,0,0,0.06);
      display: flex;
      flex-direction: column;
      gap: 16px;
      transition: transform .2s, box-shadow .2s;
    }
    .card:hover { transform: translateY(-4px); box-shadow: 0 12px 28px rgba(0,0,0,0.1); }

    .tier {
      font-size: 14px;
      font-weight: 700;
      letter-spacing: 0.08em;
      text-transform: uppercase;
      color: var(--primary);
    }
    .price {
      display: flex;
      align-items: baseline;
      gap: 4px;
    }
    .price .num   { font-size: 48px; font-weight: 800; color: var(--ink); line-height: 1; }
    .price .unit  { color: var(--muted); }

    .desc { color: var(--muted); margin: 0; line-height: 1.6; }

    .features {
      list-style: none;
      padding: 0;
      margin: 0;
      display: flex; flex-direction: column; gap: 8px;
      flex: 1;
    }
    .features li {
      position: relative;
      padding-left: 26px;
      color: var(--ink);
    }
    .features li::before {
      content: "✓";
      position: absolute;
      left: 0; top: 0;
      width: 20px; height: 20px;
      background: #d1fae5;
      color: var(--green);
      border-radius: 50%;
      display: grid; place-items: center;
      font-size: 12px;
      font-weight: 700;
    }

    .btn {
      display: block;
      width: 100%;
      padding: 12px 16px;
      border-radius: 10px;
      border: 0;
      cursor: pointer;
      font: inherit;
      font-weight: 600;
      transition: background .15s;
    }
    .btn.outline  { background: white; color: var(--primary); border: 2px solid var(--primary); }
    .btn.outline:hover  { background: var(--primary); color: white; }
    .btn.solid   { background: var(--primary); color: white; }
    .btn.solid:hover { background: #5b21b6; }
    .btn:focus-visible { outline: 3px solid var(--accent); outline-offset: 2px; }

    /* Pro card emphasis */
    .pro {
      transform: scale(1.04);
      border: 2px solid var(--accent);
      box-shadow: 0 10px 30px rgba(249, 115, 22, 0.2);
    }
    .pro:hover { transform: scale(1.04) translateY(-4px); }

    .ribbon {
      position: absolute;
      top: -14px; right: 24px;
      background: var(--accent);
      color: white;
      font-size: 11px;
      font-weight: 700;
      padding: 6px 14px;
      border-radius: 999px;
      letter-spacing: 0.04em;
      text-transform: uppercase;
    }
    .feature-pro {
      background: white;
      border-radius: var(--radius);
      padding: 28px 24px;
      display: flex; flex-direction: column; gap: 16px;
      position: relative;
    }

    @media (max-width: 800px) {
      .row { flex-direction: column; }
      .pro { transform: none; }
    }
  </style>
</head>
<body>
  <div class="wrap">
    <div class="heading">
      <h1>Plans &amp; pricing</h1>
      <p>Pick the right plan for your team.</p>
    </div>

    <div class="row">
      <article class="card">
        <div class="tier">Starter</div>
        <div class="price"><span class="num">$0</span><span class="unit">/mo</span></div>
        <p class="desc">For curious individuals.</p>
        <ul class="features">
          <li>1 project</li>
          <li>Community support</li>
          <li>Basic analytics</li>
        </ul>
        <button class="btn outline">Start free</button>
      </article>

      <article class="card feature-pro">
        <span class="ribbon">Recommended</span>
        <div class="tier" style="color: var(--accent)">Pro</div>
        <div class="price"><span class="num">$9</span><span class="unit">/mo</span></div>
        <p class="desc">For serious learners.</p>
        <ul class="features">
          <li>Unlimited projects</li>
          <li>Email support</li>
          <li>Pro workshops</li>
          <li>Advanced analytics</li>
        </ul>
        <button class="btn solid">Get Pro</button>
      </article>

      <article class="card">
        <div class="tier">Team</div>
        <div class="price"><span class="num">$29</span><span class="unit">/mo</span></div>
        <p class="desc">For schools and groups.</p>
        <ul class="features">
          <li>Everything in Pro</li>
          <li>Up to 10 seats</li>
          <li>Admin tools</li>
          <li>Priority support</li>
        </ul>
        <button class="btn outline">Contact sales</button>
      </article>
    </div>
  </div>
</body>
</html>
```

## Self-check

- [ ] Three cards in a row at desktop width.
- [ ] All cards the same height (alignment).
- [ ] Pro card visibly highlighted.
- [ ] Each card has a tier name, price, description, list, button.
- [ ] Buttons are full width.
- [ ] Mobile (<800px) they stack.
- [ ] Hover lift on each card.
- [ ] Focus styles on buttons.
- [ ] Variables used for primary/accent/ink colors.

## Bonus challenges

1. Add a "Save 20%" pill on the Pro card.
2. Add an annual/monthly toggle (only visual: just change the price from $9 to $7 with "Save 20%" subtitle).
3. Add a hover effect that makes the Pro card rotate 1° slightly.
4. Animate the price with `@property` (`<number>` type) so when you toggle annual it counts down.

## Up next

You now have card + button patterns mastered. Let's put them in a real article layout.

→ [Project 3 — Article / Blog Page](03-article-page.md)
