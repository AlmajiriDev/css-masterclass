# Lesson 14 — Links, Lists & Icons

> **The signpost analogy.** A link is a signpost with two ends. As the user, you're at one end and the destination is at the other. CSS styles these signposts depending on whether you've been there before, whether your cursor is pointing at them, whether they're being clicked, and whether the keyboard tab landed on them.

## What you'll learn

- The 4 link states: `link`, `visited`, `hover`, `active` (LVHA order).
- `text-decoration` tricks for links.
- List bullet styles and how to make horizontal lists.
- Using icon fonts and SVG icons.

## Why this matters

Links are how the web **works**. Lists are how the web organizes. Icons are how the web communicates quickly. Almost every page has all three.

## Link states

```css
a:link    { color: blue; }     /* never clicked */
a:visited { color: purple; }   /* already visited */
a:hover   { text-decoration: underline; }  /* mouse over */
a:focus   { outline: 2px solid orange; }    /* keyboard tab */
a:active  { color: red; }      /* being clicked right now */
```

> **LVHA** — **L**ink → **V**isited → **H**over → **A**ctive. Write them in this order so each one overrides the previous.

### Modern, gentle link style

```css
a {
  color: #6d28d9;
  text-decoration: none;
  border-bottom: 1px solid currentColor;
  transition: color .2s;
}
a:hover  { color: #f97316; }
a:focus-visible {
  outline: 3px solid #fde68a;
  outline-offset: 2px;
}
```

`:focus-visible` only shows the outline when the user is **keyboard navigation**. Mouse-clicking shows nothing — best of both worlds.

### Buttons styled like links

```css
.linklike {
  background: transparent;
  border: 0;
  color: #6d28d9;
  cursor: pointer;
  text-decoration: underline;
}
```

## Lists

### Bullet style

```css
ul { list-style: square; }
ul { list-style: none; }       /* remove bullets */
ol { list-style: decimal-leading-zero; }
ol { list-style: upper-roman; }
```

You can use a custom character or image:

```css
ul { list-style: "→ "; }       /* only on modern browsers */
ul { list-style: url("star.png"); }
```

### Inside spacing

Reset the default left padding:

```css
ul {
  list-style: none;
  padding-left: 0;
  margin: 0;
}
```

### Horizontal lists — for nav bars

```css
.nav {
  display: flex;        /* we'll cover Flexbox deeply in Lesson 38 */
  gap: 20px;
}
```

Horizontal lists are the seed of every navigation menu. We'll build a full nav in Lesson 31.

## Icons

### Icon fonts (Font Awesome etc.)

```html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">

<i class="fa-solid fa-house"></i>
```

Pros: easy. Cons: gigantic CSS files, often overkill.

### SVG icons — best practice

You can paste an SVG directly:

```html
<svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor">
  <path d="M12 2L2 12h3v8h14v-8h3z"/>
</svg>
```

Inline SVGs can be styled with `fill="currentColor"` and inherit the parent's color — meaning `color: red` in CSS makes the icon red.

### A simple home SVG

```html
<a class="home-link">
  <svg viewBox="0 0 24 24" width="20" height="20" fill="currentColor" aria-hidden="true">
    <path d="M12 3l9 8h-3v9h-5v-6h-2v6H6v-9H3z"/>
  </svg>
  Home
</a>
```

```css
.home-link {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  color: #6d28d9;
  text-decoration: none;
  font-weight: 600;
}
.home-link svg { transition: transform .2s; }
.home-link:hover svg { transform: translateX(2px); }
```

The icon nudges forward when you hover. Cute.

## Try it yourself

Create `practice/lesson14.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 14</title>
  <style>
    body {
      font-family: system-ui, sans-serif;
      max-width: 700px;
      margin: 30px auto;
      padding: 20px;
      background: #fffbf5;
      color: #1e293b;
    }

    a {
      color: #6d28d9;
      text-decoration: none;
    }
    a:hover {
      color: #f97316;
    }
    a:focus-visible {
      outline: 3px solid #fde68a;
      outline-offset: 2px;
    }
    a {
      border-bottom: 1px solid currentColor;
    }

    /* list reset */
    ul.clean, ol.clean { padding: 0; margin: 0; list-style: none; }

    /* horizontal list */
    .nav {
      display: flex;
      gap: 20px;
      padding: 0;
      margin: 0 0 16px;
      list-style: none;
    }
    .nav a {
      border: 0;
      padding: 6px 10px;
      border-radius: 6px;
    }
    .nav a:hover {
      background: #ede9fe;
      color: #f97316;
    }

    /* custom bullet */
    .star-list {
      list-style: none;
      padding: 0;
    }
    .star-list li {
      padding: 4px 0 4px 28px;
      position: relative;
    }
    .star-list li::before {
      content: "★";
      color: #f97316;
      position: absolute;
      left: 0;
      top: 4px;
    }

    /* inline SVG icon link */
    .icon-link {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      border: 0;
      font-weight: 600;
    }
    .icon-link svg {
      transition: transform .2s;
    }
    .icon-link:hover svg {
      transform: translateX(2px);
    }
  </style>
</head>
<body>

  <h1>Links, lists & icons</h1>

  <h2>1. Link states</h2>
  <p>
    <a href="#"             >Never visited</a> &middot;
    <a href="#this-one"     >A regular link</a> &middot;
    <a href="https://developer.mozilla.org">Visit MDN once</a> &middot;
    <a href="#" tabindex="0">Try Tab+Enter</a>
  </p>
  <p>Hover each link. Tab into the last one to see focus-visible.</p>

  <h2>2. Navigation list</h2>
  <ul class="nav">
    <li><a href="#">Home</a></li>
    <li><a href="#">About</a></li>
    <li><a href="#">Blog</a></li>
    <li><a href="#">Contact</a></li>
  </ul>

  <h2>3. Star list (custom bullet)</h2>
  <ul class="star-list">
    <li>CSS basics</li>
    <li>Box model</li>
    <li>Flexbox</li>
    <li>Grid</li>
  </ul>

  <h2>4. Icons</h2>
  <a class="icon-link" href="#">
    <svg viewBox="0 0 24 24" width="20" height="20" fill="currentColor" aria-hidden="true">
      <path d="M12 3l9 8h-3v9h-5v-6h-2v6H6v-9H3z"/>
    </svg>
    Home
  </a>
  <a class="icon-link" href="#" style="color:#f97316">
    <svg viewBox="0 0 24 24" width="20" height="20" fill="currentColor" aria-hidden="true">
      <path d="M20 4H4v2l8 5 8-5V4zm0 4l-8 5-8-5v12h16V8z"/>
    </svg>
    Email
  </a>

  <h2>5. Ordered list with different style</h2>
  <ol style="list-style: upper-roman">
    <li>Plan</li>
    <li>Style</li>
    <li>Test</li>
  </ol>
</body>
</html>
```

Open it. Tab through with the keyboard — see how `:focus-visible` only appears for keyboard users.

## Common mistakes

- **Hover styles without focus styles** — keyboard users get no feedback.
- **Underline removal with no replacement** — links must be obviously clickable.
- **No accessible name on icon links** — a screen reader sees only an SVG. Add `aria-label` or visible text.
- **Inline SVG without `aria-hidden`** if the icon is decorative.

## Debugging tips

- Tab through your page to test focus styles. The CSS `:focus-visible` is your friend.
- View source of a site you love and copy the icon strategy.

## Quick quiz

1. What's the correct order of link pseudo-classes for a stable style?
2. How do you remove bullets from a list?
3. How do you make a bullet character appear via pseudo-element?

<details>
<summary>Answers</summary>

1. `:link`, `:visited`, `:hover`, `:active` (LVHA).
2. `list-style: none;`
3. With `::before` and `content: "★";`

</details>

## Mini challenge

Build a 3-item "feature list" with a green checkmark before each item. Inline SVG or `::before` with a unicode check.

## What's next

We can dress text. Now we move into the **box model** — the secret why everything in CSS is a box.

→ [Lesson 15 — The Box Model](15-box-model.md)
