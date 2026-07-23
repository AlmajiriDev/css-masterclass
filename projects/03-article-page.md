# Project 3 — Article / Blog Page

> A long-form article page with proper typography, a hero image, an author block, and a sidebar with related links. This project teaches **reading-width typography** — the part of CSS beginners rarely get right.

## What you'll build

A single article page:

- Hero area with title, subtitle, author info, date.
- A wide hero image (or gradient placeholder) with optional "cover" treatment.
- The article body — paragraphs, subheadings, an image inside, a blockquote.
- A "share" bar at the end.
- A sidebar with "Related articles" cards (mini-cards).
- Sticky table of contents on the left (desktop only).

On mobile, the sidebar moves below the article and the TOC hides.

## Lessons this builds on

- [Lesson 09 — Text](../lessons/09-text.md)
- [Lesson 10 — Fonts](../lessons/10-fonts.md)
- [Lesson 16 — Width & max-width](../lessons/16-height-width.md)
- [Lesson 17 — Padding](../lessons/17-padding.md)
- [Lesson 19 — Display](../lessons/19-display.md)
- [Lesson 29 — Tables (for share counts row)](../lessons/29-tables.md)
- [Lesson 31 — Navigation, sticky](../lessons/31-navigation.md)
- [Lesson 38 — Flexbox](../lessons/38-flexbox.md)
- [Lesson 40 — Grid](../lessons/40-grid.md)
- [Lesson 43 — Responsive](../lessons/43-responsive.md)

## Specs

| Element | Rule |
|---------|------|
| Body | background cream, max reading width 720px centered |
| Hero image | full bleed (escape the article width), 100vw, max-height 480px |
| Article title | clamp(2rem, 5vw, 3.5rem), tight line-height |
| Article subtitle | slightly muted, larger than body |
| Body paragraphs | 17–18px font, line-height 1.8, max measure 65ch |
| H2 headings | margin-top 2.5rem, line-height 1.2, custom underline |
| Image inside | `width: 100%`, rounded corners 12px, centered |
| Blockquote | left border 4px accent, larger font, italic |
| Author block | flex row, 48px avatar, name + role |
| Sticky TOC | position sticky, hidden on mobile |
| Sidebar | mini-cards, 16px gap |

## Step-by-step plan

1. **HTML skeleton**: header, article (with hero), body, sidebar, share bar.
2. **Base styles**: typography defaults (line-height, color, max-width).
3. **Hero image**: use a CSS gradient as placeholder, full bleed.
4. **Title & subtitle**: clamp() for fluid typography.
5. **Body styling**: spacing between paragraphs (`p + p { margin-top: 1em }` or use `gap`).
6. **H2 underline**: `border-bottom: 3px solid accent; padding-bottom: 8px`.
7. **Blockquote**: large, indented, accent border.
8. **Author block**: flex with avatar + text.
9. **TOC**: position sticky on desktop, hidden below 800px.
10. **Sidebar**: grid of 3 mini-cards.
11. **Share bar**: flex row of buttons.
12. **Responsive**: hide TOC, sidebar stacks below article.

## Reference solution

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>The Art of Reading Width — CSS Masterclass</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600;800&family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
  <style>
    *, *::before, *::after { box-sizing: border-box; }

    :root {
      --primary:  #6d28d9;
      --accent:   #f97316;
      --ink:      #1e293b;
      --muted:    #6b7280;
      --bg:       #fffbf5;
      --surface:  #ffffff;
      --border:   #e5e7eb;
      --radius:   14px;
    }

    body {
      margin: 0;
      background: var(--bg);
      color: var(--ink);
      font-family: "Inter", system-ui, sans-serif;
      font-size: 17px;
      line-height: 1.7;
    }

    /* HEADER */
    header {
      padding: 24px 32px;
      border-bottom: 1px solid var(--border);
      background: white;
    }
    .brand { font-weight: 800; font-size: 20px; color: var(--primary); }

    /* HERO (full bleed) */
    .hero {
      width: 100vw;
      margin-left: calc(50% - 50vw);
      min-height: 320px;
      background:
        linear-gradient(135deg, rgba(109,40,217,0.7), rgba(249,115,22,0.7)),
        linear-gradient(135deg, #6d28d9, #f97316);
      color: white;
      display: grid; place-items: end start;
      padding: 40px 32px;
    }
    .hero h1 {
      font-family: "Playfair Display", Georgia, serif;
      font-size: clamp(2rem, 5vw, 3.5rem);
      line-height: 1.1;
      max-width: 800px;
      margin: 0;
    }

    /* LAYOUT */
    .layout {
      display: grid;
      gap: 32px;
      grid-template-columns: 1fr;
      max-width: 1100px;
      margin: 0 auto;
      padding: 32px;
    }
    @media (min-width: 900px) {
      .layout {
        grid-template-columns: 220px minmax(0, 1fr) 280px;
      }
    }

    /* TOC */
    .toc {
      position: sticky;
      top: 24px;
      align-self: start;
      display: none;
    }
    @media (min-width: 900px) { .toc { display: block; } }
    .toc h4 { font-size: 11px; letter-spacing: 0.1em; text-transform: uppercase; color: var(--muted); margin: 0 0 12px; }
    .toc ul { list-style: none; padding: 0; margin: 0; }
    .toc li { margin: 6px 0; }
    .toc a { color: var(--ink); text-decoration: none; font-size: 14px; line-height: 1.5; }
    .toc a:hover { color: var(--primary); }

    /* ARTICLE */
    article {
      max-width: 65ch;
      margin: 0 auto;
    }
    .meta {
      color: var(--muted);
      font-size: 14px;
      margin-bottom: 24px;
    }
    .meta strong { color: var(--ink); }

    .lede {
      font-size: 1.2rem;
      color: #555;
      line-height: 1.7;
      margin: 0 0 32px;
      font-style: italic;
    }

    article h2 {
      font-family: "Playfair Display", Georgia, serif;
      font-size: 1.6rem;
      line-height: 1.2;
      margin: 2.5rem 0 1rem;
      border-bottom: 3px solid var(--accent);
      padding-bottom: 8px;
    }
    article p {
      margin: 0 0 1em;
    }
    article p + p { margin-top: 1em; }

    article figure {
      margin: 32px 0;
    }
    article figure img,
    article figure .placeholder {
      width: 100%;
      aspect-ratio: 16/9;
      border-radius: var(--radius);
      background: linear-gradient(135deg, #6d28d9, #f97316);
      object-fit: cover;
      display: block;
    }
    figcaption {
      color: var(--muted);
      font-size: 14px;
      text-align: center;
      margin-top: 8px;
    }

    article blockquote {
      margin: 32px 0;
      padding: 8px 24px;
      border-left: 4px solid var(--accent);
      font-family: "Playfair Display", Georgia, serif;
      font-style: italic;
      font-size: 1.4rem;
      line-height: 1.5;
      color: var(--ink);
    }

    /* AUTHOR */
    .author {
      display: flex;
      gap: 16px;
      align-items: center;
      padding: 16px 0;
      margin: 32px 0 24px;
      border-top: 1px solid var(--border);
      border-bottom: 1px solid var(--border);
    }
    .author .avatar {
      width: 48px; height: 48px;
      border-radius: 50%;
      background: linear-gradient(135deg, #6d28d9, #f97316);
    }
    .author .name { font-weight: 700; }
    .author .role { color: var(--muted); font-size: 14px; }

    /* SHARE */
    .share {
      display: flex;
      gap: 8px;
      margin: 24px 0;
    }
    .share a {
      flex: 1;
      text-align: center;
      padding: 10px;
      border-radius: 10px;
      background: white;
      border: 1px solid var(--border);
      text-decoration: none;
      color: var(--ink);
      font-size: 14px;
      font-weight: 600;
    }
    .share a:hover { background: var(--primary); color: white; }

    /* SIDEBAR */
    aside h4 {
      font-size: 11px; letter-spacing: 0.1em; text-transform: uppercase;
      color: var(--muted);
      margin: 0 0 12px;
    }
    .related {
      display: grid;
      gap: 16px;
    }
    .mini {
      background: white;
      border-radius: var(--radius);
      padding: 14px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.06);
    }
    .mini h5 { font-family: "Playfair Display", serif; margin: 8px 0 6px; line-height: 1.2; }
    .mini .pic {
      aspect-ratio: 16/9;
      border-radius: 8px;
      background: linear-gradient(135deg, #6d28d9, #f97316);
    }
  </style>
</head>
<body>
  <header><div class="brand">CSS Masterclass</div></header>

  <div class="hero">
    <h1>The Art of Reading Width</h1>
  </div>

  <div class="layout">
    <nav class="toc" aria-label="In this article">
      <h4>In this article</h4>
      <ul>
        <li><a href="#measure">Measure</a></li>
        <li><a href="#contrast">Contrast</a></li>
        <li><a href="#hierarchy">Hierarchy</a></li>
      </ul>
    </nav>

    <article>
      <p class="meta">By <strong>Hauwa Bello</strong> · July 20, 2026 · 8 min read</p>
      <p class="lede">Reading on screens is different from reading on paper. Here is how designers make long articles feel short.</p>

      <h2 id="measure">Measure</h2>
      <p>Studies suggest the ideal line length on screens is between 50 and 75 characters. CSS lets us lock this with one rule: <code>max-width: 65ch;</code>. The browser holds that line for us.</p>
      <p>Lines that are too long make eyes lose their place when they wrap. Lines that are too short make readers feel interrupted. The 65ch sweet spot feels right for most body text at 17–18px.</p>

      <figure>
        <div class="placeholder" style="background:linear-gradient(135deg,#10b981,#0ea5e9)"></div>
        <figcaption>A measurement demo: 65ch fits this sentence on most desktop screens.</figcaption>
      </figure>

      <h2 id="contrast">Contrast</h2>
      <p>Body text needs at least 4.5:1 contrast against its background. Pitch-black on pure-white is too sharp; deep gray on cream is calmer.</p>

      <blockquote>"Typography is the craft of endowing human language with a durable visual form."</blockquote>

      <h2 id="hierarchy">Hierarchy</h2>
      <p>Three levels — title, subtitle, body — are enough. Add more only when the article demands it. Don't let type sizes compete for attention.</p>

      <div class="author">
        <div class="avatar"></div>
        <div>
          <div class="name">Hauwa Bello</div>
          <div class="role">Editor · CSS Masterclass</div>
        </div>
      </div>

      <div class="share">
        <a href="#">Share on X</a>
        <a href="#">Email</a>
        <a href="#">Copy link</a>
      </div>
    </article>

    <aside>
      <h4>Related</h4>
      <div class="related">
        <div class="mini">
          <div class="pic"></div>
          <h5>The 4-page hero</h5>
          <p style="color: var(--muted); font-size: 14px; margin: 0;">When dramatic landing pages work.</p>
        </div>
        <div class="mini">
          <div class="pic" style="background: linear-gradient(135deg,#f97316,#dc2626);"></div>
          <h5>Reading on phones</h5>
          <p style="color: var(--muted); font-size: 14px; margin: 0;">Why mobile typography is different.</p>
        </div>
      </div>
    </aside>
  </div>
</body>
</html>
```

## Self-check

- [ ] Article body max-width is 65ch.
- [ ] H2 has the orange underline.
- [ ] Hero image escapes the article width.
- [ ] Blockquote has the accent border and italic style.
- [ ] TOC is sticky on desktop, hidden on mobile.
- [ ] Sidebar stacks below on mobile.
- [ ] Author block + share bar present.
- [ ] Body font is 17px+ with line-height 1.7+.

## Bonus

1. Add a "back to top" floating button (fixed, position).
2. Make article images show as figures with caption alignment.
3. Add a dark mode using `prefers-color-scheme`.

## Up next

Navigation, but with all the polish: dropdown, mobile menu, sticky behavior.

→ [Project 4 — Accessible Navigation Menu](04-navigation.md)
