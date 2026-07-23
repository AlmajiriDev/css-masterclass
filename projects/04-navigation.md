# Project 4 — Accessible Navigation Menu

> The nav is the part every visitor uses within the first 3 seconds. Build a navigation that **works on desktop, mobile, with keyboard, with screen reader, and with a focus indicator**. This is your accessibility checkpoint.

## What you'll build

A header with:

- Brand/logo on the left.
- Primary nav links (Home / Projects / Blog / Contact).
- A "Get started" CTA button on the right.
- A dropdown on one nav item (Projects).
- A mobile hamburger menu using `<details>` (CSS-only).
- Sticky behavior on scroll.
- Visible focus rings and keyboard support throughout.

## Lessons this builds on

- [Lesson 14 — Links &amp; lists](../lessons/14-links-lists-icons.md)
- [Lesson 21 — Position](../lessons/21-position.md)
- [Lesson 22 — Z-index](../lessons/22-z-index.md)
- [Lesson 26 — Pseudo-classes](../lessons/26-pseudo-classes.md)
- [Lesson 31 — Navigation bars](../lessons/31-navigation.md)
- [Lesson 33 — Dropdowns](../lessons/33-dropdowns-tooltips.md)
- [Lesson 38 — Flexbox](../lessons/38-flexbox.md)
- [Lesson 44 — Media queries](../lessons/44-media-queries.md)
- [Lesson 49 — Accessibility](../lessons/49-accessibility-supports.md)

## Specs

| Element | Rule |
|---------|------|
| Header | full width, sticky, white background, shadow, padding 14px 24px |
| Brand | left-aligned, bold, 20px, primary color |
| Nav links | horizontal flex row, gap 8px |
| Active link | filled purple pill, white text |
| Dropdown | appears on hover and on `:focus-within`, animated |
| CTA button | primary, desktop only right-aligned with margin-left: auto |
| Mobile | hide nav under 800 px, show hamburger |
| Hamburger | pure CSS, no JS, summary/details pattern |
| Focus rings | 3px orange on `:focus-visible` everywhere |

## Step-by-step plan

1. **HTML**: header with brand, primary nav (with one details/summary for dropdown), CTA.
2. **Sticky header**: position sticky, top 0, z-index 10, white background, shadow.
3. **Flex row**: brand left, nav center, CTA right (`margin-left: auto`).
4. **Nav links**: pill style, hover background, active is filled.
5. **Dropdown**: details/summary inside nav with sub-links styled as menu.
6. **Mobile hamburger**: details/summary with a `::before` icon and accordion menu.
7. **Responsive**: hide `nav.primary` on mobile, show hamburger, both flip at 800 px.
8. **Focus rings**: `:focus-visible` everywhere interactive.
9. **Skip link**: visually hidden `<a>` at the very top for keyboard users.

## Reference solution

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Nav project</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    html { scroll-behavior: smooth; }

    body {
      margin: 0;
      font-family: system-ui, sans-serif;
      background: #fffbf5;
      color: #1e293b;
    }

    /* Skip link */
    .skip {
      position: absolute;
      top: -100px;
      left: 16px;
      background: #6d28d9;
      color: white;
      padding: 8px 16px;
      border-radius: 8px;
      z-index: 100;
      transition: top .2s;
    }
    .skip:focus-visible { top: 16px; outline: 3px solid #f97316; outline-offset: 2px; }

    /* HEADER */
    .header {
      position: sticky;
      top: 0;
      z-index: 50;
      background: rgba(255, 255, 255, 0.95);
      backdrop-filter: blur(8px);
      border-bottom: 1px solid #e5e7eb;
      box-shadow: 0 1px 6px rgba(0,0,0,0.05);
    }
    .header-inner {
      max-width: 1100px;
      margin: 0 auto;
      padding: 14px 24px;
      display: flex;
      align-items: center;
      gap: 16px;
    }
    .brand {
      font-weight: 800;
      font-size: 20px;
      color: #6d28d9;
      text-decoration: none;
    }

    /* PRIMARY NAV (desktop) */
    .primary {
      margin-left: auto;
    }
    .primary > ul {
      display: flex;
      gap: 4px;
      align-items: center;
      list-style: none;
      margin: 0;
      padding: 0;
    }
    .primary a {
      padding: 8px 14px;
      border-radius: 8px;
      text-decoration: none;
      color: #1e293b;
      font-weight: 600;
      font-size: 15px;
    }
    .primary a:hover { background: #ede9fe; }
    .primary a.active {
      background: #6d28d9;
      color: white;
    }
    .primary a:focus-visible {
      outline: 3px solid #f97316;
      outline-offset: 2px;
    }

    /* Dropdown */
    .dropdown { position: relative; }
    .dropdown summary {
      list-style: none;
      cursor: pointer;
      padding: 8px 14px;
      border-radius: 8px;
      font-weight: 600;
      font-size: 15px;
    }
    .dropdown summary::-webkit-details-marker { display: none; }
    .dropdown summary:hover { background: #ede9fe; }
    .dropdown summary::after {
      content: " ▾";
      color: #6b7280;
      font-size: 11px;
    }
    .dropdown[open] > summary { background: #ede9fe; }
    .dropdown > ul {
      position: absolute;
      top: calc(100% + 8px);
      left: 0;
      background: white;
      border-radius: 12px;
      box-shadow: 0 8px 20px rgba(0,0,0,0.12);
      padding: 6px;
      list-style: none;
      min-width: 200px;
      border: 1px solid #eee;
      margin: 0;
    }
    .dropdown > ul a {
      display: block;
      padding: 8px 12px;
      border-radius: 6px;
    }

    /* CTA button */
    .cta {
      background: #f97316;
      color: white;
      padding: 10px 18px;
      border-radius: 10px;
      text-decoration: none;
      font-weight: 700;
      transition: transform .1s, background .15s;
    }
    .cta:hover  { background: #ea580c; }
    .cta:active { transform: translateY(1px); }
    .cta:focus-visible { outline: 3px solid #fde68a; outline-offset: 2px; }

    /* MOBILE */
    .mobile { display: none; }

    @media (max-width: 800px) {
      .primary { display: none; }
      .cta { display: none; }
      .mobile { display: block; flex: 1; }
      .mobile details summary {
        list-style: none;
        cursor: pointer;
        padding: 8px 14px;
        border-radius: 8px;
        font-weight: 700;
        font-size: 22px;
        color: #6d28d9;
        text-align: right;
        user-select: none;
      }
      .mobile details summary::-webkit-details-marker { display: none; }
      .mobile details[open] summary::before { content: "✕"; }
      .mobile details:not([open]) summary::before { content: "☰"; }
      .mobile details > ul {
        position: fixed;
        top: 65px;
        left: 16px;
        right: 16px;
        background: white;
        border-radius: 14px;
        box-shadow: 0 8px 20px rgba(0,0,0,0.12);
        padding: 16px;
        list-style: none;
        margin: 0;
        display: flex; flex-direction: column; gap: 8px;
      }
      .mobile a {
        padding: 12px 14px;
        border-radius: 10px;
        text-decoration: none;
        color: #1e293b;
        font-weight: 600;
      }
      .mobile a:hover { background: #ede9fe; }
    }

    /* Fake page content for sticky demo */
    main { max-width: 720px; margin: 0 auto; padding: 24px; }
    section { min-height: 60vh; }
    section:nth-of-type(odd) { background: #fef3c7; }
    section:nth-of-type(even) { background: #fde68a; }
    section { padding: 40px 24px; border-radius: 14px; margin-bottom: 16px; }
  </style>
</head>
<body>
  <a class="skip" href="#main">Skip to content</a>

  <header class="header">
    <div class="header-inner">
      <a class="brand" href="#">▲ CSS</a>

      <details class="mobile" aria-label="Mobile menu">
        <summary aria-label="Open menu"></summary>
        <ul>
          <li><a href="#">Home</a></li>
          <li><a href="#">Projects</a></li>
          <li><a href="#">Blog</a></li>
          <li><a href="#">Contact</a></li>
          <li><a class="cta" style="display:inline-block;text-align:center;margin-top:8px" href="#">Get started</a></li>
        </ul>
      </details>

      <nav class="primary" aria-label="Primary">
        <ul>
          <li><a href="#" class="active">Home</a></li>
          <li><a href="#">About</a></li>
          <li>
            <details class="dropdown">
              <summary>Projects</summary>
              <ul>
                <li><a href="#">Profile card</a></li>
                <li><a href="#">Pricing</a></li>
                <li><a href="#">Landing page</a></li>
              </ul>
            </details>
          </li>
          <li><a href="#">Blog</a></li>
          <li><a href="#">Contact</a></li>
        </ul>
      </nav>

      <a class="cta" href="#">Get started</a>
    </div>
  </header>

  <main id="main">
    <h1>Sticky nav demo</h1>
    <p>Scroll the page. The nav stays put. Tab into it — focus rings appear. Resize below 800px — the hamburger appears.</p>
    <section></section>
    <section></section>
    <section></section>
    <section></section>
  </main>
</body>
</html>
```

## Self-check

- [ ] Brand on the left, CTA on the right (desktop).
- [ ] One nav item has a dropdown that opens with mouse and keyboard.
- [ ] Header sticks to top on scroll.
- [ ] Below 800px, nav hides, hamburger appears.
- [ ] Hamburger opens a full mobile menu.
- [ ] Active link has the filled style.
- [ ] `:focus-visible` outlines are visible.
- [ ] Skip link works (press Tab on a fresh load).
- [ ] Page maintains basic accessibility.

## Bonus

1. Add a search box in the middle of the nav (input with magnifier icon).
2. Add a "light/dark" toggle that uses CSS variables.
3. Animate the dropdown opening (slide down + fade in).

## Up next

Now the design recreation — match the look of a given mockup using all your CSS knowledge.

→ [Project 5 — Design Recreation Challenge](05-design-recreation.md)
