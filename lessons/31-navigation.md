# Lesson 31 — Navigation Bars

> **The dashboard analogy.** A nav bar is the dashboard of your site — it tells the user where they are and where they can go. CSS lets you build horizontal, vertical, sticky, responsive, and accessible nav menus using mostly Flexbox.

## What you'll learn

- Horizontal and vertical nav layouts.
- Active link styling.
- Mobile (responsive) navigation patterns.
- Sticky and off-canvas variants.

## Why this matters

Navigation is the part of every page. Get it right and everything else feels easy to use.

## The HTML pattern

```html
<nav aria-label="Primary">
  <ul class="nav">
    <li><a href="#" class="active">Home</a></li>
    <li><a href="#">About</a></li>
    <li><a href="#">Blog</a></li>
    <li><a href="#">Contact</a></li>
  </ul>
</nav>
```

Semantic HTML, `aria-label`, class for the active page.

## Horizontal — the modern pattern (Flexbox)

```css
.nav {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  gap: 4px;
}
.nav a {
  display: inline-block;
  padding: 10px 14px;
  border-radius: 8px;
  text-decoration: none;
  color: #1e293b;
  font-weight: 600;
}
.nav a:hover { background: #ede9fe; }
.nav a.active {
  background: #6d28d9;
  color: white;
}
.nav a:focus-visible {
  outline: 3px solid #fde68a;
  outline-offset: 2px;
}
```

## "Logo left, links right"

```css
.bar {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.links { display: flex; gap: 8px; }
.spacer { flex: 1; }
```

Or with auto margin:

```css
.brand { /* default */ }
.spacer { margin-left: auto; display: flex; gap: 8px; }
```

## Sticky nav

```css
header {
  position: sticky;
  top: 0;
  background: white;
  padding: 10px 24px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.06);
  z-index: 50;
}
```

## Mobile pattern (responsive)

The simplest pattern is "wrap so it doesn't fit":

```css
.nav {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
}
```

On mobile that gracefully wraps. On desktop it's a single row.

For a hidden mobile menu (hamburger), use the "checkbox hack" or JavaScript-free `:target` trick:

```html
<input type="checkbox" id="menu-toggle">
<label for="menu-toggle" class="hamburger">☰</label>
<nav class="menu">…links…</nav>
```

```css
#menu-toggle { display: none; }
.hamburger   { display: none; cursor: pointer; }

@media (max-width: 600px) {
  .hamburger { display: block; }
  .menu {
    display: none;
  }
  #menu-toggle:checked + .hamburger + .menu {
    display: block;
  }
}
```

We won't lean on this — it's a teaching example. In real apps use a tiny JS handler.

## A real "active link" pattern

When the user is on `/about`, that link should look active. CSS only solution (no JS):

```html
<body class="page-about">
  <nav>
    <a href="/" class="home">Home</a>
    <a href="/about" class="about">About</a>
  </nav>
</body>
```

```css
.page-home   .home,
.page-about  .about,
.page-blog   .blog { background: #6d28d9; color: white; }
```

Use the body class or a template engine to set the active state.

## Try it yourself

Create `practice/lesson31.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 31 — nav</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body {
      font-family: system-ui, sans-serif;
      background: #fffbf5;
      margin: 0;
    }
    h1, h2 { padding: 0 24px; color: #1e293b; }
    h1 { color: #6d28d9; }

    /* 1. Horizontal nav */
    header.bar {
      display: flex;
      align-items: center;
      gap: 14px;
      padding: 14px 24px;
      background: white;
      box-shadow: 0 2px 10px rgba(0,0,0,0.06);
      position: sticky;
      top: 0;
      z-index: 50;
    }
    .brand { font-weight: 800; font-size: 20px; color: #6d28d9; }
    nav.primary {
      margin-left: auto;
    }
    .primary ul {
      display: flex;
      gap: 4px;
      list-style: none;
      margin: 0;
      padding: 0;
    }
    .primary a {
      display: inline-block;
      padding: 8px 14px;
      border-radius: 8px;
      text-decoration: none;
      color: #1e293b;
      font-weight: 600;
    }
    .primary a:hover { background: #ede9fe; }
    .primary a.active { background: #6d28d9; color: white; }
    .primary a:focus-visible { outline: 3px solid #fde68a; outline-offset: 2px; }

    /* fake long page content for sticky demo */
    main { padding: 0 24px 60px; }
    .placeholder { height: 600px; }
    .placeholder:nth-of-type(odd) { background: #fef3c7; }
    .placeholder:nth-of-type(even) { background: #fde68a; }

    /* 2. Vertical nav (sidebar) */
    .layout {
      display: grid;
      grid-template-columns: 200px 1fr;
      gap: 20px;
      padding: 30px 24px;
    }
    .sidebar ul {
      list-style: none;
      margin: 0; padding: 0;
      display: grid;
      gap: 4px;
    }
    .sidebar a {
      display: block;
      padding: 10px 12px;
      border-radius: 8px;
      text-decoration: none;
      color: #1e293b;
    }
    .sidebar a:hover { background: #ede9fe; }
    .sidebar a.active { background: #ede9fe; color: #6d28d9; }

    @media (max-width: 600px) {
      .layout { grid-template-columns: 1fr; }
      .primary ul { flex-wrap: wrap; }
    }
  </style>
</head>
<body>

  <header class="bar">
    <div class="brand">Logo</div>
    <nav class="primary" aria-label="Primary">
      <ul>
        <li><a href="#" class="active">Home</a></li>
        <li><a href="#">About</a></li>
        <li><a href="#">Blog</a></li>
        <li><a href="#">Contact</a></li>
      </ul>
    </nav>
  </header>

  <main>
    <h1>Sticky navbar demo</h1>
    <p>Scroll the page — the bar stays at the top.</p>
    <div class="placeholder"></div>
    <div class="placeholder"></div>
  </main>

  <h2>Sidebar / vertical nav</h2>
  <div class="layout">
    <aside class="sidebar">
      <ul>
        <li><a href="#" class="active">Dashboard</a></li>
        <li><a href="#">Reports</a></li>
        <li><a href="#">Settings</a></li>
      </ul>
    </aside>
    <section>
      <h3>Content area</h3>
      <p>Resize the window — below 600px wide, the sidebar moves below the content.</p>
    </section>
  </div>
</body>
</html>
```

## Common mistakes

- **Dropping `<nav>` semantic** — use `<nav>` even if it just contains a `<ul>`.
- **Hidden focus rings** — `:focus-visible` saves you.
- **Mobile menu that doesn't trap focus** — needs JS in real apps. CSS-only is limited.

## Quick quiz

1. Center a logo on the **left** and nav links on the **right** of a header.
2. Make the header stick to the top when scrolling.
3. How do you style the active link in pure CSS?

<details>
<summary>Answers</summary>

1. Header = `display: flex`. Logo = first child. Nav = `margin-left: auto`.
2. `position: sticky; top: 0;`
3. Either add an `.active` class per page (via template/server) or use a body class and descendant selector.

</details>

## Mini challenge

Build a responsive nav: logo + 4 links. On mobile, links wrap to 2 rows. On desktop, links sit on the right.

## What's next

We built a nav. Now we wrap it in a useful component — buttons & cards.

→ [Lesson 32 — Buttons & Cards](32-buttons-cards.md)
