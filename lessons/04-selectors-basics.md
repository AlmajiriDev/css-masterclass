# Lesson 04 — CSS Selectors (the basics)

> **The classroom analogy.** Imagine a school. The principal wants to say something to *all the students* (tag selector), or only the *girls* (class selector), or only *Hauwa who sits at the back* (id selector). Selectors are how CSS picks who to talk to.

## What you'll learn

- The 5 most-used selector types.
- When to use a tag, a class, or an id.
- How `*` (universal) works.
- How to group selectors.

## Why this matters

Every styling decision in CSS starts with a selector. If you choose the wrong selector, your style hits the wrong elements — or no elements at all. Mastering selectors is mastering 60% of CSS.

## The 5 basic selector types

### 1. Universal selector — `*`

```css
* {
  box-sizing: border-box;
}
```

Hits **every** element. Useful as a reset (more on that in later lessons). Don't use it for actual styling — it's expensive.

### 2. Tag (type) selector

```css
h1 { color: purple; }
p  { line-height: 1.6; }
a  { text-decoration: none; }
```

Hits every element of that tag. Great for **defaults** like paragraph line-height or link colors.

### 3. Class selector — `.classname`

```css
.card { padding: 16px; border-radius: 12px; }
.button { background: orange; color: white; }
```

Hits every element with that `class="..."`. **This is your workhorse.** Use classes for reusable components: 3 different elements with `class="card"` will all be styled identically.

### 4. ID selector — `#id`

```css
#main { max-width: 800px; margin: auto; }
```

Hits the **single** element with that `id`. IDs are like names — there can only be one of each. Use sparingly.

> **Rule of thumb:** prefer **classes** for styling. Use IDs only when you need a unique anchor (for example, to link to a section).

### 5. Grouping selectors — `,`

```css
h1, h2, h3 {
  font-family: Georgia, serif;
  color: #2c1810;
}
```

Hits **any** of them. Saves you from writing the same rule three times.

## What's a "declaration block" again?

```css
selector, another-selector {   /* selectors */
  property: value;              /* declaration */
  another-property: value;
}
```

## Try it yourself

Create `practice/lesson04.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 04</title>
  <style>
    * { box-sizing: border-box; }

    body {
      font-family: system-ui, sans-serif;
      background: #fffbf5;
      padding: 30px;
    }

    /* tag selector */
    h1 { color: #6d28d9; }
    p  { color: #333; line-height: 1.6; }

    /* group selector */
    h1, h2, h3 { font-family: Georgia, serif; }

    /* class selector */
    .card {
      background: #ede9fe;
      padding: 16px;
      border-radius: 12px;
      margin: 10px 0;
    }

    /* id selector */
    #hero {
      background: linear-gradient(135deg, #fef3c7, #fde68a);
      padding: 30px;
      border-radius: 12px;
    }

    .button {
      display: inline-block;
      background: #f97316;
      color: white;
      padding: 10px 18px;
      border-radius: 8px;
      text-decoration: none;
    }
  </style>
</head>
<body>
  <section id="hero">
    <h1>Welcome to Lesson 04</h1>
    <p>Selectors are CSS superpowers.</p>
    <a class="button" href="#cards">Scroll down</a>
  </section>

  <h2>Cards</h2>
  <div class="card">Card 1</div>
  <div class="card">Card 2</div>
  <div class="card">Card 3</div>
</body>
</html>
```

Open it. Notice how:
- Every paragraph looks the same (tag selector).
- The hero section has its own color (id selector).
- All 3 cards share the same look (class selector).
- The buttons also use a class.

## Common mistakes

- **Using an id for every element.** It's allowed but ugly. Use a class.
- **Same id twice.** IDs must be unique within a page. If you have two `#hero`, the second one is illegal HTML and your CSS will behave unpredictably.
- **Forgetting the dot on a class.** `.card` ≠ `card`.

## Quick quiz

1. Which selector would you use to style 5 `<button>` elements the same way?
2. What's wrong with `<div id="box">` and `<div id="box">` on the same page?
3. Write a group selector that styles both `h1` and `h2` with color `#6d28d9`.

<details>
<summary>Answers</summary>

1. A class: `<button class="btn">` × 5, then `.btn { ... }`.
2. Duplicate IDs. IDs must be unique on a page.
3. `h1, h2 { color: #6d28d9; }`

</details>

## Mini challenge

Inside `lesson04.html`, add a third card with `class="card highlight"` and write a rule for `.highlight` that gives it a yellow background. (We touched on multiple classes in Lesson 01 — now it's official: an element can have **many** classes, separated by spaces.)

## What's next

We just used the 5 most common selectors. Next we look at more powerful family-tree selectors: combinators and attribute selectors.

→ [Lesson 05 — Combinators & Attribute Selectors](05-selectors-advanced.md)
