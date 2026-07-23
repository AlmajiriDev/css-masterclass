# Lesson 05 — Combinators & Attribute Selectors

> **The family-tree analogy.** Combinators describe relationships in your HTML family. Is the element a **child** of another? A **sibling**? A **grandchild**? Each relationship has a selector. Attribute selectors are like *"look for everyone whose name (attribute) starts with..."*.

## What you'll learn

- The 4 combinators: descendant, child, adjacent sibling, general sibling.
- Attribute selectors: `[attr]`, `[attr="x"]`, `[attr^="x"]`, `[attr$="x"]`, `[attr*="x"]`.
- When to use a class vs an attribute selector.

## Why this matters

You will eventually need to style *"every link inside the sidebar, but not outside it"* or *"every input that is required"*. Tag selectors can't do that. Combinators and attribute selectors can.

## Combinators

Imagine this HTML:

```html
<nav>
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="/about">About</a></li>
  </ul>
</nav>
<main>
  <a href="/blog">Blog</a>
</main>
```

We want links inside the `<nav>` to look different from the one in `<main>`.

### 1. Descendant combinator — `A B` (just a space)

```css
nav a {
  color: white;
  text-decoration: none;
}
```

Reads: *"any `a` inside `nav`."* It doesn't matter how deep — grandchildren, great-grandchildren, all of them.

### 2. Child combinator — `A > B`

```css
ul > li {
  list-style: none;
}
```

Reads: *"direct child `li` of `ul`."* Only direct children, not nested ones.

### 3. Adjacent sibling — `A + B`

```css
h1 + p {
  font-size: 1.2rem;
  font-weight: bold;
}
```

Reads: *"a `<p>` that **immediately follows** an `<h1>`."* A small but useful trick for the intro paragraph after a heading.

### 4. General sibling — `A ~ B`

```css
h1 ~ p {
  color: gray;
}
```

Reads: *"any `<p>` that is a sibling of `<h1>` and **after** it."* (Doesn't matter if there are things in between.)

> **Memory aid:**
> - `space` = "inside somewhere"
> - `>` = "directly the child of"
> - `+` = "immediately after"
> - `~` = "anywhere after"

## Attribute selectors

Attribute selectors target HTML attributes — `href`, `type`, `src`, `disabled`, `required`, custom `data-*`, anything.

### Basic — `[attr]`

```css
a[target] { color: tomato; }
```

Any `<a>` that has a `target` attribute.

### Exact match — `[attr="value"]`

```css
input[type="text"] { padding: 10px; }
input[type="submit"] { background: orange; }
```

Most common form-styling pattern. We'll use this heavily in Lesson 30.

### Starts with — `[attr^="value"]`

```css
a[href^="https"] { color: green; }
```

All external (https) links. Useful for showing users "this link leaves our site".

### Ends with — `[attr$="value"]`

```css
a[href$=".pdf"] { font-weight: bold; }
```

Links pointing to PDFs.

### Contains — `[attr*="value"]`

```css
[class*="btn"] { padding: 8px; }
```

Any element whose `class` attribute contains `btn` somewhere — picks up `btn`, `big-btn`, `btn-primary`, etc.

### Insensitive — `[attr="value" i]`

```css
a[href$=".PDF" i] { /* picks up .pdf and .PDF */ }
```

Adds ` i` to make the match case-insensitive.

## Try it yourself

Create `practice/lesson05.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 05</title>
  <style>
    body { font-family: system-ui, sans-serif; padding: 30px; }

    /* descendant combinator */
    nav a {
      color: white;
      text-decoration: none;
      padding: 8px 14px;
      background: #6d28d9;
      border-radius: 6px;
      display: inline-block;
    }

    /* child combinator */
    nav > ul { padding: 0; }
    nav > ul > li { display: inline-block; margin: 6px; }

    /* adjacent sibling */
    h2 + p { color: #555; font-style: italic; }

    /* attribute selectors */
    a[href^="https"] { background: green; }
    a[href$=".pdf"]   { background: #dc2626; }
    a[href^="mailto"] { background: #f59e0b; }
    input[required]   { border: 2px solid orange; }
    [data-tag]        { padding: 4px 10px; background: #ede9fe; border-radius: 999px; }

    .btn-primary,
    .btn-secondary,
    .btn-large { padding: 8px 14px; border-radius: 6px; }

    /* attribute contains */
    [class*="btn-"] { font-weight: bold; }
  </style>
</head>
<body>
  <h1>Lesson 05 — Combinators & Attributes</h1>

  <nav>
    <ul>
      <li><a href="/home">Home</a></li>
      <li><a href="/about">About</a></li>
      <li><a href="https://example.com">External</a></li>
    </ul>
  </nav>

  <h2>Subsection</h2>
  <p>This paragraph follows the h2 directly — see how it's italic?</p>
  <p>This one is a later sibling, not adjacent.</p>

  <h2>Different links</h2>
  <p>
    <a class="btn-primary"  href="/home">Primary</a>
    <a class="btn-secondary" href="https://example.com">External</a>
    <a class="btn-large"     href="brochure.pdf">Download PDF</a>
    <a                      href="mailto:hi@example.com">Email us</a>
  </p>

  <h2>Required field</h2>
  <input type="text" placeholder="Not required">
  <input type="text" placeholder="Required" required>

  <h2>Custom data attribute</h2>
  <span data-tag>Featured</span>
  <span>Regular</span>
</body>
</html>
```

Open it. See how different selectors produce different effects on the same elements.

## Class vs attribute — when to use which?

- Use **class** when grouping by *intent* (`.button`, `.card`).
- Use **attribute** when grouping by *property* (form `[type=...]`, language `[lang]`).
- They can also work together: `a.btn[href^="https"]`.

## Common mistakes

- `A,B` ≠ `A B`. The first is grouping (OR), the second is descendant (inside).
- Forgetting that attribute selectors need **quotes** around values when they contain special characters. Always quote them anyway.

## Debugging tips

- Use [w3schools CSS selector tester](https://www.w3schools.com/cssref/trysel.asp) for fast experiments.
- In DevTools, right-click an element → Copy → Copy selector — see what selector the browser suggests.

## Quick quiz

1. What's the difference between `A B` and `A > B`?
2. Write a selector that matches every `<a>` whose `href` ends with `.pdf`.
3. Write a selector that matches every `<input>` whose `type` is `email`.

<details>
<summary>Answers</summary>

1. `A B` = any descendant (any depth). `A > B` = **direct** child only.
2. `a[href$=".pdf"]`
3. `input[type="email"]`

</details>

## Mini challenge

In `lesson05.html`, add a third paragraph **inside the `<nav>`** (yes, between the `<ul>`'s `<li>`s or right after the `<ul>`) and write a combinator rule that styles **only** that paragraph with bold text. Make sure the existing paragraphs outside `<nav>` are not affected.

## What's next

Now we know *who* to style. Next we decide *what color* — the colorful world of CSS colors.

→ [Lesson 06 — Colors in CSS](06-colors.md)
