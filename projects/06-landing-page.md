# Project 6 — Business Landing Page

> A landing page is the front door of a business. In this project you'll combine **everything** you've learned — typography, color, layout, responsive design, components — into one multi-section responsive page.

## What you'll build

A single-page marketing site for a fictional product (we'll call it "Brandname"). It has:

1. **Top nav** with logo, links, and CTA button.
2. **Hero** section with a headline, subhead, primary CTA, secondary CTA, and a hero visual.
3. **Features** row: 3 cards with icons and short descriptions.
4. **Showcase** section: a screenshot or big visual with bullet points beside it.
5. **Stats** row: 4 numbers in a row (e.g., "10k+ users").
6. **Pricing** row: 3 cards (you built these in project 2 — recycle!).
7. **Testimonials**: 3 quotes from imaginary customers.
8. **FAQ**: a few `<details>` items.
9. **CTA banner**: final push with email input + button.
10. **Footer** with link columns.

It must be **fully responsive**: at desktop width it has multi-column rows; on mobile it stacks.

## Lessons this builds on

Essentially every project so far — these become building blocks.

- [Project 1 — Profile card](01-profile-card.md) for avatars and pills
- [Project 2 — Pricing cards](02-pricing-cards.md) for the pricing row
- [Project 4 — Navigation](04-navigation.md) for the top nav
- [Lesson 38 — Flexbox](../lessons/38-flexbox.md), [Lesson 40 — Grid](../lessons/40-grid.md), [Lesson 43 — Responsive](../lessons/43-responsive.md), [Lesson 45 — clamp()](../lessons/45-math-functions.md), [Lesson 47 — Variables](../lessons/47-variables.md)

## Specs (section by section)

| Section | Rule |
|---------|------|
| Container | max-width 1100px, `margin-inline: auto`, fluid padding |
| Top nav | sticky, white, shadow (reuse project 4) |
| Hero | 2-column on desktop (text + visual), stacks on mobile. Headline uses `clamp()` for fluid size |
| Features | 3-column grid (`repeat(auto-fit, minmax(220px, 1fr))`) |
| Showcase | 2-column row, visual + bullets |
| Stats | 4 numbers in a flex row, big font, gradient numbers |
| Pricing | 3 cards (Project 2 recipe) |
| Testimonials | 3 cards with quote, name, avatar |
| FAQ | centered column of `<details>` |
| CTA banner | full-bleed gradient, white text, max content width 600px |
| Footer | 4-column grid (logo + 3 link columns), bottom legal line |

## Step-by-step plan

1. **HTML skeleton** with all sections.
2. **Tokens** at `:root`: colors, spacing, radii, max-width.
3. **Reset** and base typography.
4. **Top nav** — copy from Project 4 (simplified).
5. **Hero** with fluid headline + dual CTA.
6. **Features** row with icon-based cards.
7. **Showcase** section — text + visual.
8. **Stats** row with big numbers.
9. **Pricing** — copy from Project 2.
10. **Testimonials** — quote style card.
11. **FAQ** — `<details>` items.
12. **CTA banner** with email input + button.
13. **Footer** with link columns.
14. **Responsive sweeps** — fix any issues at 360 px wide.

## Reference (compact) solution

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Brandname — landing page</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }

    :root {
      --primary: #6d28d9;
      --accent:  #f97316;
      --ink:     #1e293b;
      --muted:   #6b7280;
      --bg:      #fffbf5;
      --surface: #ffffff;
      --border:  #e5e7eb;
      --radius:  16px;
      --shadow:  0 4px 14px rgba(0,0,0,0.08);
    }
    body {
      margin: 0;
      font-family: system-ui, sans-serif;
      background: var(--bg);
      color: var(--ink);
    }
    .container { max-width: 1100px; margin-inline: auto; padding: clamp(1rem, 4vw, 4rem); }
    h1, h2, h3 { line-height: 1.15; margin: 0 0 12px; }
    p  { line-height: 1.7; color: #444; }
    a  { color: var(--primary); text-decoration: none; }
    a:hover { text-decoration: underline; }

    /* NAV */
    nav.top {
      position: sticky; top: 0; z-index: 50;
      background: rgba(255,255,255,0.95);
      backdrop-filter: blur(8px);
      border-bottom: 1px solid var(--border);
    }
    .top-inner { max-width: 1100px; margin: 0 auto; padding: 14px 24px;
                 display: flex; align-items: center; gap: 16px; }
    .brand { font-weight: 800; font-size: 20px; color: var(--primary); }
    .links { margin-left: auto; display: flex; gap: 8px; }
    .links a { padding: 8px 12px; border-radius: 8px; color: var(--ink); font-weight: 600; }
    .links a:hover { background: #ede9fe; }
    .cta-btn { background: var(--primary); color: white; padding: 10px 18px; border-radius: 10px; font-weight: 600; }
    .cta-btn:hover { background: #5b21b6; text-decoration: none; }

    /* HERO */
    .hero {
      display: grid;
      grid-template-columns: 1fr;
      gap: 32px;
      align-items: center;
      padding: clamp(2rem, 6vw, 6rem) clamp(1rem, 4vw, 4rem);
    }
    @media (min-width: 800px) { .hero { grid-template-columns: 1.1fr 1fr; } }
    .hero h1 {
      font-size: clamp(2rem, 6vw, 4.5rem);
      letter-spacing: -0.02em;
      line-height: 1.05;
    }
    .hero h1 span { color: var(--accent); }
    .hero p { font-size: 1.1rem; max-width: 50ch; margin: 16px 0 24px; }
    .hero-buttons { display: flex; gap: 12px; flex-wrap: wrap; }
    .btn-primary {
      background: var(--primary); color: white;
      padding: 14px 24px; border-radius: 10px;
      font-weight: 700; text-decoration: none;
    }
    .btn-primary:hover { background: #5b21b6; }
    .btn-secondary {
      background: white; color: var(--primary);
      border: 2px solid var(--border);
      padding: 14px 24px; border-radius: 10px;
      font-weight: 700; text-decoration: none;
    }
    .btn-secondary:hover { border-color: var(--primary); }
    .hero-visual {
      aspect-ratio: 1/1;
      border-radius: 20px;
      background:
        radial-gradient(circle at 70% 30%, #fde68a 0 60px, transparent 70px),
        linear-gradient(135deg, var(--primary), var(--accent));
      box-shadow: 0 30px 60px rgba(109,40,217,0.25);
    }

    /* GENERIC SECTIONS */
    section { padding: clamp(2rem, 5vw, 5rem) clamp(1rem, 4vw, 4rem); }
    .section-title { text-align: center; max-width: 600px; margin: 0 auto 32px; }
    .section-title h2 { font-size: clamp(1.6rem, 3vw, 2.5rem); }
    .section-title p { color: var(--muted); }

    /* FEATURES */
    .features {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 16px;
      max-width: 1100px;
      margin: 0 auto;
    }
    .feature {
      background: white;
      padding: 24px;
      border-radius: var(--radius);
      box-shadow: var(--shadow);
    }
    .feature .icon {
      width: 48px; height: 48px;
      background: var(--primary); color: white;
      border-radius: 12px;
      display: grid; place-items: center;
      font-size: 22px;
      margin-bottom: 12px;
    }
    .feature h3 { margin: 0 0 8px; }

    /* SHOWCASE */
    .showcase {
      display: grid;
      grid-template-columns: 1fr;
      gap: 32px;
      align-items: center;
      max-width: 1100px; margin: 0 auto;
    }
    @media (min-width: 800px) { .showcase { grid-template-columns: 1fr 1fr; } }
    .showcase-visual {
      aspect-ratio: 4/3;
      border-radius: var(--radius);
      background: linear-gradient(135deg, #10b981, #0ea5e9);
    }
    .showcase ul { padding: 0; list-style: none; }
    .showcase li { padding: 12px 0; border-bottom: 1px solid var(--border); display: flex; gap: 8px; }
    .showcase li:last-child { border-bottom: 0; }
    .showcase li::before {
      content: "✓";
      display: grid; place-items: center;
      width: 24px; height: 24px;
      background: #d1fae5; color: #059669;
      border-radius: 50%;
      flex-shrink: 0; font-size: 12px; font-weight: 700;
    }

    /* STATS */
    .stats {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
      gap: 16px;
      max-width: 900px; margin: 0 auto;
      text-align: center;
    }
    .stat .num {
      font-size: clamp(2rem, 5vw, 3rem);
      font-weight: 800;
      background: linear-gradient(135deg, var(--primary), var(--accent));
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
      line-height: 1;
    }
    .stat .label { color: var(--muted); margin-top: 6px; }

    /* PRICING — recycle from project 2 */
    .pricing {
      display: grid; gap: 16px;
      grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
      max-width: 1100px; margin: 0 auto;
    }
    .plan {
      background: white;
      padding: 24px; border-radius: var(--radius);
      box-shadow: var(--shadow);
    }
    .plan.featured {
      border: 2px solid var(--accent);
      box-shadow: 0 12px 30px rgba(249,115,22,0.2);
    }
    .plan .tier { font-weight: 700; color: var(--muted); letter-spacing: 0.05em; text-transform: uppercase; font-size: 13px; }
    .plan .num  { font-size: 36px; font-weight: 800; margin: 8px 0; }
    .plan ul    { padding: 0; list-style: none; }
    .plan ul li { padding: 6px 0; color: #555; }
    .plan .btn  { display: inline-block; width: 100%; text-align: center; padding: 12px; border-radius: 10px; background: var(--primary); color: white; font-weight: 600; margin-top: 12px; }
    .plan.featured .btn { background: var(--accent); }
    .plan .btn:hover { opacity: .9; text-decoration: none; }

    /* TESTIMONIALS */
    .testimonials {
      display: grid; gap: 16px;
      grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
      max-width: 1100px; margin: 0 auto;
    }
    .testimonial {
      background: white;
      padding: 24px;
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      display: flex; flex-direction: column; gap: 16px;
    }
    .testimonial .quote { font-style: italic; color: #555; line-height: 1.6; margin: 0; }
    .testimonial .who   { display: flex; align-items: center; gap: 10px; }
    .testimonial .who img,
    .testimonial .pic {
      width: 40px; height: 40px;
      border-radius: 50%;
      background: linear-gradient(135deg, var(--primary), var(--accent));
    }
    .testimonial .who strong { display: block; }
    .testimonial .who span { color: var(--muted); font-size: 14px; }

    /* FAQ */
    .faq {
      max-width: 720px;
      margin: 0 auto;
      display: flex; flex-direction: column; gap: 8px;
    }
    .faq details {
      background: white;
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 12px 16px;
    }
    .faq summary { cursor: pointer; font-weight: 700; }
    .faq p { margin: 12px 0 0; }

    /* CTA BANNER */
    .cta-banner {
      background: linear-gradient(135deg, var(--primary), var(--accent));
      color: white;
      border-radius: var(--radius);
      padding: 48px 32px;
      text-align: center;
      max-width: 1100px;
      margin: 0 auto;
    }
    .cta-banner h2 { font-size: clamp(1.6rem, 3vw, 2.2rem); }
    .cta-banner form {
      max-width: 480px;
      margin: 24px auto 0;
      display: flex; gap: 8px;
      flex-wrap: wrap;
    }
    .cta-banner input {
      flex: 1;
      min-width: 200px;
      padding: 12px 16px;
      border: 0;
      border-radius: 10px;
      font: inherit;
    }
    .cta-banner button {
      background: var(--ink);
      color: white;
      border: 0;
      padding: 12px 24px;
      border-radius: 10px;
      font-weight: 700;
      cursor: pointer;
    }

    /* FOOTER */
    footer {
      background: var(--ink);
      color: #cbd5e1;
      padding: 48px 24px;
      margin-top: 64px;
    }
    footer .grid {
      max-width: 1100px;
      margin: 0 auto;
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
      gap: 32px;
    }
    footer h4 { color: white; margin: 0 0 12px; }
    footer ul { list-style: none; padding: 0; margin: 0; }
    footer li { margin: 6px 0; }
    footer a  { color: #cbd5e1; }
    footer .legal {
      max-width: 1100px;
      margin: 32px auto 0;
      padding-top: 16px;
      border-top: 1px solid #334155;
      font-size: 14px;
      text-align: center;
    }
  </style>
</head>
<body>
  <nav class="top">
    <div class="top-inner">
      <a class="brand" href="#">▲ Brandname</a>
      <div class="links">
        <a href="#features">Features</a>
        <a href="#pricing">Pricing</a>
        <a href="#faq">FAQ</a>
      </div>
      <a class="cta-btn" href="#">Get started</a>
    </div>
  </nav>

  <header class="hero container">
    <div>
      <h1>The <span>smartest</span> way to ship CSS.</h1>
      <p>Stop fighting cascade wars. Build accessible, responsive interfaces in a fraction of the time — with the patterns you already love.</p>
      <div class="hero-buttons">
        <a class="btn-primary" href="#">Get started free</a>
        <a class="btn-secondary" href="#pricing">See pricing</a>
      </div>
    </div>
    <div class="hero-visual"></div>
  </header>

  <section id="features">
    <div class="section-title">
      <h2>Built for the way you build.</h2>
      <p>Three reasons teams choose Brandname.</p>
    </div>
    <div class="features">
      <div class="feature"><div class="icon">★</div><h3>Fast</h3><p>Built-in patterns keep your stylesheets tiny and your pages snappy.</p></div>
      <div class="feature"><div class="icon">★</div><h3>Accessible</h3><p>ARIA, focus rings, and reduced motion — all by default.</p></div>
      <div class="feature"><div class="icon">★</div><h3>Responsive</h3><p>Mobile-first utilities and breakpoints for every device.</p></div>
    </div>
  </section>

  <section style="background: #fef3c7;">
    <div class="showcase">
      <div class="showcase-visual"></div>
      <div>
        <h2>Everything you need</h2>
        <p>Components, layouts, typography — all unified under one design language.</p>
        <ul>
          <li>Reusable component CSS</li>
          <li>Full responsive grid system</li>
          <li>Theme variables out of the box</li>
          <li>Optimized for performance</li>
        </ul>
      </div>
    </div>
  </section>

  <section>
    <div class="section-title"><h2>Trusted by builders</h2></div>
    <div class="stats">
      <div class="stat"><div class="num">10k+</div><div class="label">Users</div></div>
      <div class="stat"><div class="num">98%</div><div class="label">Satisfaction</div></div>
      <div class="stat"><div class="num">50M</div><div class="label">CSS lines saved</div></div>
      <div class="stat"><div class="num">4.9</div><div class="label">Star rating</div></div>
    </div>
  </section>

  <section style="background: #f5f3ff;">
    <div class="section-title"><h2>Simple pricing</h2><p>Start free, upgrade when you need.</p></div>
    <div class="pricing">
      <div class="plan"><div class="tier">Starter</div><div class="num">$0</div><ul><li>1 project</li><li>Community support</li></ul><a class="btn" href="#">Choose</a></div>
      <div class="plan featured"><div class="tier">Pro</div><div class="num">$9</div><ul><li>Unlimited projects</li><li>Email support</li></ul><a class="btn" href="#">Choose</a></div>
      <div class="plan"><div class="tier">Team</div><div class="num">$29</div><ul><li>Up to 10 seats</li><li>Priority support</li></ul><a class="btn" href="#">Choose</a></div>
    </div>
  </section>

  <section>
    <div class="section-title"><h2>Loved by developers</h2></div>
    <div class="testimonials">
      <div class="testimonial"><p class="quote">"Brandname is my default starting point for every project."</p><div class="who"><div class="pic"></div><div><strong>Hauwa</strong><span>Designer, Lagos</span></div></div></div>
      <div class="testimonial"><p class="quote">"We shipped our marketing site in two days."</p><div class="who"><div class="pic"></div><div><strong>Aisha</strong><span>Engineer, Abuja</span></div></div></div>
      <div class="testimonial"><p class="quote">"The accessibility defaults alone saved us a sprint."</p><div class="who"><div class="pic"></div><div><strong>Bilal</strong><span>PM, Kano</span></div></div></div>
    </div>
  </section>

  <section>
    <div class="section-title"><h2>Frequently asked</h2></div>
    <div class="faq">
      <details><summary>What does it cost?</summary><p>There's a free tier forever. Paid tiers start at $9/mo.</p></details>
      <details><summary>Can I cancel anytime?</summary><p>Yes. Cancel from your account in two clicks.</p></details>
      <details><summary>Is there a student discount?</summary><p>Yes — 50% off with a verified school email.</p></details>
    </div>
  </section>

  <section style="background: #f5f3ff;">
    <div class="cta-banner">
      <h2>Ready to ship faster?</h2>
      <p>Start free in under a minute.</p>
      <form onsubmit="event.preventDefault(); alert('Thanks!');">
        <input type="email" placeholder="you@example.com" required>
        <button type="submit">Get started</button>
      </form>
    </div>
  </section>

  <footer>
    <div class="grid">
      <div>
        <h4>▲ Brandname</h4>
        <p>Build beautiful UIs faster.</p>
      </div>
      <div><h4>Product</h4><ul><li><a href="#">Features</a></li><li><a href="#">Pricing</a></li><li><a href="#">Roadmap</a></li></ul></div>
      <div><h4>Company</h4><ul><li><a href="#">About</a></li><li><a href="#">Blog</a></li><li><a href="#">Careers</a></li></ul></div>
      <div><h4>Legal</h4><ul><li><a href="#">Privacy</a></li><li><a href="#">Terms</a></li><li><a href="#">Cookies</a></li></ul></div>
    </div>
    <p class="legal">© 2026 Brandname. All rights reserved.</p>
  </footer>
</body>
</html>
```

## Self-check

- [ ] Sections appear in the order described.
- [ ] Layout is responsive: stacks on phones, multi-column on desktop.
- [ ] Top nav sticks on scroll.
- [ ] Buttons and links have hover and focus states.
- [ ] Color contrast meets 4.5:1 for body text.
- [ ] No horizontal scroll on phones.
- [ ] Visuals use CSS gradients (no external images).
- [ ] FAQ is keyboard-accessible (`<details>` works out of the box).

## Bonus

1. Add `prefers-reduced-motion: reduce` overrides.
2. Add an animated number counter using `@property`.
3. Make the hero visual a 3D flip card that shows a second "feature" face on hover.

## Up next

The final boss: a personal portfolio site that ties everything together.

→ [Project 7 — Personal Portfolio](07-portfolio.md)
