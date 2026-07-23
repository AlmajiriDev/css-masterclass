# Lesson 27 — Pseudo-elements

> **The puppet-string analogy.** Pseudo-elements let you reach into the imaginary parts of an element — the first letter, the first line, the start of the content, the end. They are not real DOM nodes, but the browser lets you style them as if they were.

## What you'll learn

- `::before`, `::after` — most-used pseudo-elements.
- `::first-letter` and `::first-line`.
- `::placeholder` and `::selection`.
- The `content` property.

## Why this matters

Pseudo-elements are the secret of every polished UI. Icons inside buttons without extra HTML. Decorative dots. Tooltips. Drop caps. **Decoration without bloat.**

## The `::before` / `::after` duo

These add decorative content **before or after** the element's real content.

```css
h2::before {
  content: "→ ";
  color: #f97316;
}
.external::after {
  content: " ↗";
  color: #6d28d9;
  font-size: 0.8em;
}
```

Three rules:

1. They **must** have `content`. Even an empty `content: ""`.
2. They are inline by default. Make them `display: inline-block` or `block` to size them.
3. They are invisible to screen readers by default. Add visual but no extra meaning (use real text for information).

## `content` accepts:

```css
content: "";              /* empty (just for shape) */
content: "•";            /* a string */
content: attr(data-info); /* reads from data-* attribute */
content: counter(my-counter);
content: url("icon.svg");
```

The `attr()` function is gold — it lets CSS read the HTML:

```html
<a data-info="(PDF)" href="...">Brochure</a>
```

```css
a[data-info]::after {
  content: " " attr(data-info);
  font-size: 0.8em;
  color: gray;
}
```

## Common patterns

### Decorative icon before a heading

```css
h2::before {
  content: "✦";
  color: #f97316;
  margin-right: 6px;
}
```

### Clearfix (modern)

You can replace the `.clearfix` hack with:

```css
.clearfix::after { content: ""; display: block; clear: both; }
```

### Notification badge

```css
.btn { position: relative; }
.btn::after {
  content: "";
  position: absolute;
  top: -4px; right: -4px;
  width: 8px; height: 8px;
  background: #dc2626;
  border-radius: 50%;
}
```

## `::first-letter`

```css
p::first-letter {
  font-size: 4em;
  float: left;
  line-height: 0.8;
  margin: 6px 8px 0 0;
  color: #6d28d9;
  font-weight: bold;
}
```

The classic drop cap. Only the **first letter** of the first formatted line of a block element.

## `::first-line`

```css
.lede::first-line {
  font-weight: bold;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}
```

Styles the first line of text as displayed. Reflows when the line breaks change.

## Form pseudo-elements

```css
input::placeholder { color: #aaa; font-style: italic; }
input::focus { /* we use this */ }
input::value { /* not real */ }   /* doesn't exist */

::selection { background: #fde68a; }   /* selection highlight */
```

Yes, you can style the user's selected text globally.

## Tooltip pattern (no extra markup)

```css
.has-tip { position: relative; cursor: help; }
.has-tip::after {
  content: attr(data-tip);
  position: absolute;
  bottom: 125%;
  left: 50%;
  transform: translateX(-50%);
  background: #1e293b;
  color: white;
  padding: 6px 10px;
  border-radius: 6px;
  white-space: nowrap;
  opacity: 0;
  pointer-events: none;
  transition: opacity .2s;
}
.has-tip:hover::after { opacity: 1; }
```

```html
<span class="has-tip" data-tip="I'm a tooltip!">Hover me</span>
```

## Try it yourself

Create `practice/lesson27.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 27 — pseudo-elements</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body {
      font-family: system-ui, sans-serif;
      max-width: 720px;
      margin: 30px auto;
      padding: 20px;
      background: #fffbf5;
      color: #1e293b;
      line-height: 1.7;
    }

    /* decorative star */
    h2::before {
      content: "✦";
      color: #f97316;
      margin-right: 6px;
    }

    /* external link marker */
    a[href^="http"]::after {
      content: " ↗";
      color: #6d28d9;
      font-size: 0.8em;
    }

    /* data-info label */
    .doc[data-info]::after {
      content: " (" attr(data-info) ")";
      color: #999;
      font-size: 0.8em;
    }

    /* drop cap */
    .dropcap::first-letter {
      font-size: 4em;
      float: left;
      line-height: 0.8;
      margin: 6px 8px 0 0;
      color: #6d28d9;
      font-weight: bold;
    }

    /* notification badge */
    .bell {
      position: relative;
      display: inline-block;
      font-size: 28px;
      padding: 8px 16px;
      background: #ede9fe;
      border-radius: 999px;
    }
    .bell::after {
      content: "";
      position: absolute;
      top: 4px; right: 4px;
      width: 10px; height: 10px;
      background: #dc2626;
      border-radius: 50%;
    }

    /* tooltip */
    .tip { position: relative; cursor: help; border-bottom: 1px dotted #6d28d9; }
    .tip::after {
      content: attr(data-tip);
      position: absolute;
      bottom: 125%;
      left: 50%;
      transform: translateX(-50%);
      background: #1e293b;
      color: white;
      padding: 6px 10px;
      border-radius: 6px;
      white-space: nowrap;
      font-size: 0.85rem;
      opacity: 0;
      pointer-events: none;
      transition: opacity .2s;
    }
    .tip:hover::after { opacity: 1; }

    /* selection */
    ::selection { background: #fde68a; color: #1e1b3a; }

    .placeholder-input::placeholder { color: #aaa; font-style: italic; }

    /* custom list bullet */
    ul.custom { list-style: none; padding: 0; }
    ul.custom li { padding: 6px 0 6px 24px; position: relative; }
    ul.custom li::before {
      content: "✓";
      position: absolute;
      left: 0;
      color: #059669;
      font-weight: 700;
    }
  </style>
</head>
<body>
  <h1>Pseudo-elements</h1>

  <h2>Decorative stars before headings</h2>
  <p>Notice the orange star next to every h2.</p>

  <h2>External links</h2>
  <p>
    Visit <a href="https://developer.mozilla.org">MDN</a> for reference,
    or read <a href="/local" data-info="local">a local article</a>.
  </p>

  <h2>Drop cap</h2>
  <p class="dropcap">A drop cap is the giant first letter at the start of an article. With `::first-letter`, you don't need extra markup — the browser automatically styles the very first letter of the first formatted line.</p>

  <h2>Notification badge</h2>
  <span class="bell">🔔</span>

  <h2>Tooltip</h2>
  <p>Hover over <span class="tip" data-tip="CSS tooltip, no JS!">this text</span> to see a tooltip.</p>

  <h2>Custom checkbox list</h2>
  <ul class="custom">
    <li>Item one</li>
    <li>Item two</li>
    <li>Item three</li>
  </ul>

  <h2>Try selecting text on this page</h2>
  <p>The selection color is custom. Try selecting text everywhere — the highlight will be a custom warm color.</p>

  <p><input class="placeholder-input" placeholder="I have italic gray placeholder text"></p>
</body>
</html>
```

## Common mistakes

- **Forgetting `content: ""`** — `::before` / `::after` won't render without it.
- **Adding real info with `::before`** — screen readers skip pseudo-element content unless you set `aria-hidden` or similar. Use real DOM for important content.
- **`::first-letter` only on the first letter of the first line** — won't affect any second letter.

## Debugging tips

- In DevTools → Elements, pseudo-elements appear as `::before` and `::after` sub-nodes under their parent. Inspect them directly.

## Quick quiz

1. What property must every `::before` have?
2. What does `content: attr(data-info)` do?
3. How do you style selected text globally?

<details>
<summary>Answers</summary>

1. `content` (at least an empty string).
2. Reads the `data-info` attribute from the element and uses it as content.
3. `::selection { ... }`.

</details>

## Mini challenge

Build a "card with notification dot in the top-right corner" using only `::after` (no extra HTML for the dot).

## What's next

Now we learn the meta-skill — when specificity wins and when it doesn't, and the role of `!important`.

→ [Lesson 28 — !important & Debugging Specificity](28-important-debug.md)
