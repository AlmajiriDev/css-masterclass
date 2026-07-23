# Project 7 — Personal Portfolio

> The final project. A 6-section responsive portfolio site that showcases you, your work, and your skills. It's the **culmination** of everything you learned in this course. Build it once, host it anywhere, share with everyone.

## What you'll build

A 1-page portfolio website (or with a separate `projects.html` link) containing:

1. **Top nav** with logo, links (Home / About / Projects / Contact), and a "Hire me" CTA.
2. **Hero** with name, role, tagline, CTA buttons.
3. **About / skills** section with a short bio and a skills grid.
4. **Featured projects** grid with 6 cards (recycle from earlier projects).
5. **Experience / timeline** section — vertical list of jobs/roles.
6. **Testimonial / quote** from an imaginary client.
7. **Contact form** (we built forms in lesson 30 — reuse!).
8. **Footer** with social links and a "back to top" arrow.

## Lessons this builds on

All of them. Specifically:

- [Project 6 — Landing page](06-landing-page.md) patterns
- [Project 1 — Profile card](01-profile-card.md) for the avatar / about section
- [Project 2 — Pricing cards](02-pricing-cards.md) for project cards
- [Project 3 — Article page](03-article-page.md) typography patterns
- [Project 4 — Navigation](04-navigation.md) for the top nav
- [Lessons 47–49](../lessons/47-variables.md) for tokens, accessibility, structure

## Specs (section by section)

| Section | Rule |
|---------|------|
| Nav | sticky, white, blur background, focus rings |
| Hero | left-aligned name huge (`clamp(2.5rem, 8vw, 6rem)`), kicker label, 2 CTAs |
| About | 2-column on desktop (avatar+text), text + skills list |
| Skills | 4-column grid of skill pills |
| Featured projects | 3-column card grid (auto-fit, minmax 280px) |
| Project cards | image, title, description, "View case study" link |
| Timeline | vertical with line, dots on each side |
| Testimonial | centered, big quote, name, role |
| Contact | form with email, subject, message (validation, accent focus rings) |
| Footer | 3 columns — about / links / socials |
| Theme | use light mode + dark mode via `prefers-color-scheme` |

## Step-by-step plan

1. **HTML skeleton** for all sections.
2. **Tokens** at `:root`: colors, spacing, radii, max-width, fonts.
3. **Reset** + base typography.
4. **Top nav** with sticky behavior and links to in-page anchors.
5. **Hero** with large fluid name and CTAs.
6. **About** with a CSS-gradient avatar and a skills grid.
7. **Featured projects** as 6 cards (use the same recipe you built in projects 1 and 2).
8. **Timeline** using `border-left` and `:nth-child` dots.
9. **Testimonial** block.
10. **Contact form** with accessibility (lesson 30).
11. **Footer** with socials.
12. **Polish**: dark mode, focus styles, reduced motion, "back to top" link.
13. **Responsive sweep**: 360, 600, 900, 1200 px.

## Reference solution

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Hauwa Bello — Portfolio</title>
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
      --shadow:  0 6px 20px rgba(0,0,0,0.08);
    }
    @media (prefers-color-scheme: dark) {
      :root {
        --bg: #0f172a;
        --surface: #1e1b3a;
        --ink: #f1f5f9;
        --muted: #94a3b8;
        --primary: #c084fc;
        --accent: #fb923c;
        --border: #1e293b;
        --shadow: 0 6px 20px rgba(0,0,0,0.4);
      }
    }

    @media (prefers-reduced-motion: reduce) {
      *, *::before, *::after { animation-duration: 0.01ms !important; transition-duration: 0.01ms !important; }
    }

    body {
      margin: 0;
      font-family: system-ui, sans-serif;
      background: var(--bg);
      color: var(--ink);
      transition: background .3s, color .3s;
    }
    a { color: var(--primary); text-decoration: none; }
    a:hover { text-decoration: underline; }

    /* NAV */
    nav.top {
      position: sticky; top: 0; z-index: 50;
      background: color-mix(in srgb, var(--bg) 90%, transparent);
      backdrop-filter: blur(8px);
      border-bottom: 1px solid var(--border);
    }
    .nav-inner {
      max-width: 1100px; margin: 0 auto;
      padding: 16px 24px;
      display: flex; align-items: center; gap: 16px;
    }
    .brand { font-weight: 800; font-size: 20px; color: var(--primary); }
    .links { margin-left: auto; display: flex; gap: 8px; }
    .links a {
      padding: 8px 12px; border-radius: 8px;
      color: var(--ink); font-weight: 600;
    }
    .links a:hover { background: #ede9fe; }
    .cta-btn {
      background: var(--primary); color: white;
      padding: 10px 18px; border-radius: 10px;
      font-weight: 700;
    }
    .cta-btn:hover { background: #5b21b6; text-decoration: none; }
    :focus-visible {
      outline: 3px solid var(--accent);
      outline-offset: 2px;
    }

    /* CONTAINER */
    .container { max-width: 1100px; margin-inline: auto; padding: clamp(1rem, 4vw, 4rem); }
    section { padding: clamp(2rem, 5vw, 5rem) clamp(1rem, 4vw, 4rem); }

    /* HERO */
    .hero { padding-top: clamp(3rem, 8vw, 7rem); }
    .kicker {
      display: inline-block;
      background: #ede9fe;
      color: var(--primary);
      padding: 6px 14px;
      border-radius: 999px;
      font-size: 13px;
      font-weight: 700;
      letter-spacing: 0.06em;
      margin-bottom: 16px;
    }
    .hero h1 {
      font-size: clamp(2.5rem, 8vw, 6rem);
      line-height: 1.05;
      letter-spacing: -0.02em;
      margin: 0 0 16px;
    }
    .hero h1 span { color: var(--accent); }
    .hero p.tag { font-size: clamp(1.1rem, 2vw, 1.5rem); color: var(--muted); max-width: 50ch; margin: 0 0 24px; line-height: 1.5; }
    .hero .cta-row { display: flex; gap: 12px; flex-wrap: wrap; }
    .btn-primary { background: var(--primary); color: white; padding: 14px 24px; border-radius: 12px; font-weight: 700; }
    .btn-primary:hover { background: #5b21b6; }
    .btn-secondary { background: var(--surface); color: var(--ink); padding: 14px 24px; border-radius: 12px; font-weight: 700; border: 2px solid var(--border); }
    .btn-secondary:hover { border-color: var(--primary); }

    /* ABOUT */
    .about {
      display: grid; gap: 32px;
      grid-template-columns: 1fr;
      align-items: center;
      max-width: 1000px; margin: 0 auto;
    }
    @media (min-width: 800px) { .about { grid-template-columns: 1fr 1fr; } }
    .avatar {
      aspect-ratio: 1/1;
      max-width: 360px;
      border-radius: 24px;
      background:
        radial-gradient(circle at 30% 30%, #fde68a, transparent 50%),
        radial-gradient(circle at 70% 70%, #f97316, transparent 60%),
        linear-gradient(135deg, var(--primary), var(--accent));
      box-shadow: var(--shadow);
      display: grid; place-items: center;
      color: white; font-size: 100px; font-weight: 800;
      margin: 0 auto;
    }

    /* SKILLS */
    .skills {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
      gap: 12px;
      margin-top: 16px;
    }
    .skill {
      background: var(--surface);
      border: 1px solid var(--border);
      padding: 12px;
      border-radius: 10px;
      text-align: center;
      font-weight: 600;
      font-size: 14px;
    }

    /* PROJECTS */
    .projects {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
      gap: 20px;
      max-width: 1100px; margin: 0 auto;
    }
    .project {
      background: var(--surface);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      overflow: hidden;
      display: flex; flex-direction: column;
      transition: transform .2s, box-shadow .2s;
    }
    .project:hover { transform: translateY(-4px); box-shadow: 0 14px 30px rgba(0,0,0,0.14); }
    .project .pic  { aspect-ratio: 16/9; }
    .project .body { padding: 18px 20px; flex: 1; display: flex; flex-direction: column; gap: 8px; }
    .project h3   { margin: 0; }
    .project p    { color: var(--muted); margin: 0; flex: 1; }
    .project a    { color: var(--primary); font-weight: 700; }

    /* TIMELINE */
    .timeline {
      max-width: 720px; margin: 0 auto;
      border-left: 2px solid var(--border);
      padding-left: 24px;
    }
    .timeline .item {
      position: relative;
      padding: 8px 0 24px;
    }
    .timeline .item::before {
      content: "";
      position: absolute;
      left: -32px; top: 12px;
      width: 12px; height: 12px;
      border-radius: 50%;
      background: var(--primary);
      box-shadow: 0 0 0 4px var(--bg);
    }
    .timeline h4 { margin: 0; }
    .timeline time { color: var(--muted); font-size: 14px; }
    .timeline p { margin: 6px 0 0; }

    /* TESTIMONIAL */
    .testimonial {
      max-width: 720px; margin: 0 auto;
      text-align: center;
      background: var(--surface);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      padding: 32px;
    }
    .testimonial blockquote { margin: 0 0 16px; font-size: 1.2rem; line-height: 1.6; }

    /* CONTACT */
    .contact {
      max-width: 600px;
      margin: 0 auto;
      background: var(--surface);
      padding: 32px;
      border-radius: var(--radius);
      box-shadow: var(--shadow);
    }
    form { display: grid; gap: 16px; }
    label { display: block; margin-bottom: 6px; font-weight: 600; font-size: 14px; }
    input, textarea {
      font: inherit;
      width: 100%;
      padding: 12px 16px;
      border: 1px solid var(--border);
      border-radius: 10px;
      background: var(--bg);
      color: var(--ink);
    }
    textarea { min-height: 120px; resize: vertical; }
    input:focus, textarea:focus {
      outline: 3px solid var(--accent);
      outline-offset: 0;
      border-color: var(--primary);
    }
    button[type="submit"] {
      background: var(--primary);
      color: white; border: 0;
      padding: 14px 24px;
      border-radius: 10px;
      font-weight: 700;
      cursor: pointer;
    }
    button[type="submit"]:hover { background: #5b21b6; }

    /* FOOTER */
    footer { background: var(--ink); color: #cbd5e1; padding: 40px 24px; margin-top: 64px; }
    .footer-inner {
      max-width: 1100px; margin: 0 auto;
      display: grid; gap: 32px;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    }
    footer h4 { color: white; margin: 0 0 12px; }
    footer a  { color: #cbd5e1; }
    .back-top {
      display: inline-block;
      margin-top: 16px; padding: 8px 14px;
      border: 1px solid #475569;
      border-radius: 999px;
      color: #cbd5e1;
    }
    .back-top:hover { background: var(--primary); color: white; border-color: var(--primary); text-decoration: none; }

    /* section title */
    .section-title { text-align: center; margin: 0 auto 32px; max-width: 600px; }
    .section-title h2 { font-size: clamp(1.6rem, 3vw, 2.5rem); margin: 0 0 8px; }
    .section-title p  { color: var(--muted); }
  </style>
</head>
<body>
  <nav class="top" aria-label="Primary">
    <div class="nav-inner">
      <a class="brand" href="#">▲ hauwa.dev</a>
      <div class="links">
        <a href="#about">About</a>
        <a href="#projects">Projects</a>
        <a href="#contact">Contact</a>
      </div>
      <a class="cta-btn" href="#contact">Hire me</a>
    </div>
  </nav>

  <header class="hero container">
    <span class="kicker">Available for hire</span>
    <h1>I build <span>interfaces</span> people remember.</h1>
    <p class="tag">Frontend developer in Lagos. CSS, accessibility, and a love for typography.</p>
    <div class="cta-row">
      <a class="btn-primary" href="#contact">Work with me</a>
      <a class="btn-secondary" href="#projects">See my work</a>
    </div>
  </header>

  <section id="about">
    <div class="section-title">
      <h2>About me</h2>
      <p>A short version of a long story.</p>
    </div>
    <div class="about">
      <div class="avatar">HB</div>
      <div>
        <p>I'm Hauwa — a designer-turned-developer who loves making the web feel calm and clear. I work mostly in CSS, HTML, and a sprinkle of JavaScript.</p>
        <div class="skills">
          <div class="skill">HTML</div>
          <div class="skill">CSS</div>
          <div class="skill">Responsive</div>
          <div class="skill">Accessibility</div>
          <div class="skill">Flexbox</div>
          <div class="skill">Grid</div>
          <div class="skill">Animations</div>
          <div class="skill">Design tokens</div>
        </div>
      </div>
    </div>
  </section>

  <section id="projects">
    <div class="section-title">
      <h2>Featured projects</h2>
      <p>A few things I'm proud of.</p>
    </div>
    <div class="projects">
      <article class="project">
        <div class="pic" style="background:linear-gradient(135deg,#6d28d9,#f97316)"></div>
        <div class="body">
          <h3>Brandname landing</h3>
          <p>A multi-section responsive marketing site.</p>
          <a href="#">View case study →</a>
        </div>
      </article>
      <article class="project">
        <div class="pic" style="background:linear-gradient(135deg,#10b981,#0ea5e9)"></div>
        <div class="body">
          <h3>Forest dashboard</h3>
          <p>Data UI with cards, tables, and charts.</p>
          <a href="#">View case study →</a>
        </div>
      </article>
      <article class="project">
        <div class="pic" style="background:linear-gradient(135deg,#f43f5e,#fb7185)"></div>
        <div class="body">
          <h3>Rose magazine</h3>
          <p>An editorial reading experience.</p>
          <a href="#">View case study →</a>
        </div>
      </article>
      <article class="project">
        <div class="pic" style="background:linear-gradient(135deg,#0ea5e9,#6366f1)"></div>
        <div class="body">
          <h3>Ocean library</h3>
          <p>A search-driven knowledge base.</p>
          <a href="#">View case study →</a>
        </div>
      </article>
      <article class="project">
        <div class="pic" style="background:linear-gradient(135deg,#fbbf24,#f59e0b)"></div>
        <div class="body">
          <h3>Sunrise planner</h3>
          <p>Daily tasks with a calm interface.</p>
          <a href="#">View case study →</a>
        </div>
      </article>
      <article class="project">
        <div class="pic" style="background:linear-gradient(135deg,#a855f7,#ec4899)"></div>
        <div class="body">
          <h3>Music mock app</h3>
          <p>A now-playing screen rebuild.</p>
          <a href="#">View case study →</a>
        </div>
      </article>
    </div>
  </section>

  <section>
    <div class="section-title"><h2>Experience</h2><p>How I got here.</p></div>
    <div class="timeline">
      <div class="item">
        <h4>Senior Frontend Developer</h4>
        <time>2024 – Present</time>
        <p>Leading the design system team at Studio X.</p>
      </div>
      <div class="item">
        <h4>Frontend Developer</h4>
        <time>2021 – 2024</time>
        <p>Shipped 30+ responsive sites for SMB clients.</p>
      </div>
      <div class="item">
        <h4>Junior Developer</h4>
        <time>2019 – 2021</time>
        <p>Cut my teeth on accessibility audits.</p>
      </div>
    </div>
  </section>

  <section>
    <div class="section-title"><h2>What clients say</h2></div>
    <div class="testimonial">
      <blockquote>"Hauwa turned our clunky marketing site into something we're proud to show off."</blockquote>
      <strong>Aisha — Marketing lead, Brandname</strong>
    </div>
  </section>

  <section id="contact">
    <div class="section-title"><h2>Get in touch</h2><p>Tell me about your project.</p></div>
    <div class="contact">
      <form novalidate>
        <div>
          <label for="name">Name</label>
          <input id="name" required placeholder="Your name">
        </div>
        <div>
          <label for="email">Email</label>
          <input id="email" type="email" required placeholder="you@example.com">
        </div>
        <div>
          <label for="msg">Project</label>
          <textarea id="msg" required placeholder="What's the project?"></textarea>
        </div>
        <button type="submit">Send message</button>
      </form>
    </div>
  </section>

  <footer>
    <div class="footer-inner">
      <div>
        <h4>▲ hauwa.dev</h4>
        <p>Frontend developer &amp; CSS lover.</p>
      </div>
      <div>
        <h4>Sitemap</h4>
        <a href="#about">About</a><br>
        <a href="#projects">Projects</a><br>
        <a href="#contact">Contact</a>
      </div>
      <div>
        <h4>Find me</h4>
        <a href="#">GitHub</a><br>
        <a href="#">X / Twitter</a><br>
        <a href="#">LinkedIn</a>
      </div>
    </div>
    <a class="back-top" href="#">↑ Back to top</a>
  </footer>
</body>
</html>
```

## Self-check

- [ ] All 8 sections present and ordered.
- [ ] Top nav is sticky and links go to anchors.
- [ ] Hero name is fluid (`clamp`) and very large.
- [ ] Skills grid uses `auto-fit minmax`.
- [ ] Project cards lift on hover.
- [ ] Timeline has a vertical line and dots.
- [ ] Contact form is accessible (labels, focus rings).
- [ ] Footer has multiple columns and a "back to top" link.
- [ ] Dark mode works via `prefers-color-scheme`.
- [ ] `prefers-reduced-motion` respected.
- [ ] Responsive at 360, 600, 900, 1200 px.

## Bonus

1. Add a "currently learning" pill in the hero.
2. Animate the timeline dots as you scroll (you'd need a tiny bit of JS — see `IntersectionObserver` if you want to try).
3. Add an `@property` animation on the hero kicker (rotating gradient).
4. Replace project pics with real screenshots when you build them.

## Final words

You now have:

- A complete CSS education (50 lessons + 7 projects).
- A personal portfolio you built from scratch.
- A path to Tailwind (lesson 50).

The skills in this course transfer to any frontend stack — once you know CSS, the rest of web work is just decoration.

Keep building. Keep sharing. Welcome to the community. 🌻

---

→ [Back to course home](../README.md)
