# Lesson 50 — From Pure CSS to Tailwind (The Map)

> **The translation-dictionary analogy.** Tailwind is a different *language* for the same ideas. Once you know pure CSS, you can read Tailwind by mapping concepts. This lesson is your translation dictionary — every concept, side by side.

## What you'll learn

- What Tailwind is and isn't.
- The naming patterns.
- How every CSS concept you've learned maps to a Tailwind class.
- A worked example: a card in pure CSS vs Tailwind.

## Why this matters

Tailwind won't replace CSS — your brain already knows CSS. Tailwind will **use** CSS as a different syntax. This map lets you adopt it in a weekend, not a month.

## What Tailwind is

Tailwind is a set of classnames that map to CSS rules. You write classes like `bg-purple-600 p-4 rounded-lg` in your HTML; Tailwind generates the corresponding CSS at build time.

```html
<button class="bg-purple-600 text-white px-4 py-2 rounded-lg">
  Click me
</button>
```

Equivalent to:

```css
.bg-purple-600 { background: #9333ea; }
.text-white    { color: white; }
.px-4          { padding-left: 1rem; padding-right: 1rem; }
.py-2          { padding-top: .5rem; padding-bottom: .5rem; }
.rounded-lg    { border-radius: .5rem; }
```

## How concepts map

### Selectors → existence in HTML

In pure CSS you separate HTML and CSS. In Tailwind, you write everything in `class="..."`. So selectors become **classnames** (or `data-` attributes).

```html
<!-- pure CSS -->
<button class="btn">Click</button>
<!-- .btn { ... } -->

<!-- Tailwind -->
<button class="bg-purple-600 text-white px-4 py-2 rounded-lg">Click</button>
```

### Colors

| Pure CSS | Tailwind |
|----------|----------|
| `color: #6d28d9;` | `text-purple-700` or `text-[#6d28d9]` |
| `background: #6d28d9;` | `bg-purple-700` |
| `border: 1px solid #d1d5db;` | `border border-gray-300` |

### Typography

| Pure CSS | Tailwind |
|----------|----------|
| `font-size: 16px;` | `text-base` or `text-lg` |
| `font-weight: 600;` | `font-semibold` |
| `line-height: 1.5;` | `leading-normal` |
| `letter-spacing: -0.02em;` | `tracking-tight` |
| `text-align: center;` | `text-center` |

### Spacing

| Pure CSS | Tailwind |
|----------|----------|
| `padding: 8px;` | `p-2` |
| `padding: 0 16px;` | `px-4` |
| `margin-top: 24px;` | `mt-6` |
| `margin: 0 auto;` | `mx-auto` (with width) |

### Sizing

| Pure CSS | Tailwind |
|----------|----------|
| `width: 100%;` | `w-full` |
| `max-width: 600px;` | `max-w-xl` |
| `height: 100vh;` | `h-screen` |
| `min-height: 100vh;` | `min-h-screen` |
| `aspect-ratio: 16/9;` | `aspect-video` |

### Display & layout

| Pure CSS | Tailwind |
|----------|----------|
| `display: flex;` | `flex` |
| `display: grid;` | `grid` |
| `flex-direction: column;` | `flex-col` |
| `justify-content: center;` | `justify-center` |
| `align-items: center;` | `items-center` |
| `gap: 16px;` | `gap-4` |
| `flex: 1;` | `flex-1` |

### Borders

| Pure CSS | Tailwind |
|----------|----------|
| `border: 1px solid;` | `border` |
| `border-radius: 12px;` | `rounded-xl` |
| `border-radius: 9999px;` | `rounded-full` |

### Effects

| Pure CSS | Tailwind |
|----------|----------|
| `box-shadow: 0 4px 14px rgba(0,0,0,.08);` | `shadow-md` |
| `opacity: 0.7;` | `opacity-70` |
| `filter: blur(8px);` | `blur` |

### States

Tailwind uses prefix syntax for pseudo-classes:

| Pure CSS | Tailwind |
|----------|----------|
| `:hover` | `hover:bg-purple-700` |
| `:focus-visible` | `focus-visible:outline focus-visible:outline-2` |
| `:disabled` | `disabled:opacity-50 disabled:cursor-not-allowed` |
| `:first-child` | `first:mt-0` |

### Responsive

Same prefixes, applied at breakpoints:

| Pure CSS | Tailwind |
|----------|----------|
| `@media (min-width: 768px)` | `md:` |
| `@media (min-width: 1024px)` | `lg:` |
| `@media (min-width: 1280px)` | `xl:` |

```html
<h1 class="text-2xl md:text-4xl">Title</h1>   <!-- mobile: 2xl, tablet+: 4xl -->
```

## A worked example — same card, two syntaxes

### Pure CSS

```html
<article class="card">
  <img alt="" src="photo.jpg">
  <h3>Title</h3>
  <p>Description.</p>
  <button class="btn">Read more</button>
</article>
```

```css
.card {
  background: white;
  border-radius: 12px;
  padding: 24px;
  box-shadow: 0 4px 14px rgba(0,0,0,0.08);
  max-width: 320px;
}
.card img { width: 100%; aspect-ratio: 16/9; object-fit: cover; border-radius: 8px; }
.card h3 { font-size: 18px; margin: 16px 0 6px; }
.card p { color: #555; line-height: 1.5; }
.btn {
  display: inline-block;
  background: #6d28d9;
  color: white;
  padding: 10px 18px;
  border-radius: 8px;
  font-weight: 600;
  margin-top: 12px;
}
```

### Same thing in Tailwind

```html
<article class="bg-white rounded-xl p-6 shadow-md max-w-xs">
  <img src="photo.jpg" alt="" class="w-full aspect-video object-cover rounded-md">
  <h3 class="text-lg mt-4 mb-1.5">Title</h3>
  <p class="text-gray-600 leading-relaxed">Description.</p>
  <button class="inline-block bg-purple-600 text-white px-5 py-2.5 rounded-lg font-semibold mt-3">
    Read more
  </button>
</article>
```

Same look. Different place. **The CSS knowledge transfers directly.**

## What changes when you switch

1. You stop maintaining `style.css` for visual styling (state of the page is encoded in HTML classes).
2. You use **config** (`tailwind.config.js`) to set your own tokens.
3. You use `@apply` in your CSS only for repeatable patterns.
4. You add **plugins** for animation, forms, typography, etc.

What doesn't change:

- The cascade.
- Specificity.
- Pseudo-classes & pseudo-elements.
- Animations / transitions / transforms.
- Responsive design.
- Accessibility.

## How to transition (recommended path)

1. **Build one of your existing pure-CSS projects in Tailwind.** Use the map above.
2. **Compare.** Notice the file count drops.
3. **Adopt incrementally.** Tailwind works alongside plain CSS — no need to rewrite.
4. **Read their docs** at [tailwindcss.com](https://tailwindcss.com). They explain each utility group.

## Resources to keep learning

- [tailwindcss.com/docs](https://tailwindcss.com/docs)
- [tailwindui.com](https://tailwindui.com) — paid components, free to study
- [tailwindcomponents.com](https://tailwindcomponents.com) — community examples
- [@tailwindcss on X](https://x.com/tailwindcss) — updates

## Closing thoughts

You started this course knowing HTML. Now you know:

- All CSS syntax, properties, and values.
- How to lay out any page with Flexbox and Grid.
- How to make a page responsive at any size.
- How to animate, theme, optimize, and ship.

That's a **complete** CSS education. From here, two paths:

1. **Practice** — build the 7 projects. Each one reuses 5-10 lessons.
2. **Tailwind** — translate your projects using the map above.

Either way, you have what you need.

Good luck — and welcome to the wider community of CSS developers. 🎉

## What's next?

The project folder. Start with project 1 and grow from there.

→ [Project 1 — Profile card](../projects/01-profile-card.md)
