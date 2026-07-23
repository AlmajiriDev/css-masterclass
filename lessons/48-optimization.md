# Lesson 48 — Optimization & Organization

> **The well-stocked pantry analogy.** A small project's CSS in one file is fine. A big project's CSS is chaos unless you organize it like a well-stocked pantry — everything has a place, sections are labeled, and nothing lives in the wrong shelf.

## What you'll learn

- A sensible file structure for CSS projects.
- The cascade layers (`@layer`) for explicit ordering.
- Performance tips: avoid expensive properties.
- Common debugging tools and habits.

## Why this matters

Projects that "almost work" tend to be messy underneath. After 50 lessons of pure CSS, you'll have the skills — now learn to make them survive a year of changes.

## File organization

A common, clean structure:

```
/css
  ├─ reset.css         /* universal reset */
  ├─ tokens.css        /* variables, design tokens */
  ├─ base.css          /* body, headings, links */
  ├─ utilities.css     /* .row, .stack, .center, etc. */
  ├─ components/
  │   ├─ button.css
  │   ├─ card.css
  │   └─ nav.css
  └─ pages/
      ├─ home.css
      └─ blog.css
```

Small projects: one file with section comments.

```css
/* ========================================
   CSS Masterclass — single stylesheet
   ======================================== */

/* 1. RESET */
*, *::before, *::after { box-sizing: border-box; }
…

/* 2. TOKENS */
:root { --primary: #6d28d9; … }

/* 3. BASE */
body { … }
h1   { … }

/* 4. UTILITIES */
.row { display: flex; gap: 16px; }
.stack > * + * { margin-top: 16px; }

/* 5. COMPONENTS */
.btn { … }
.card { … }

/* 6. PAGE-SPECIFIC */
.home-hero { … }
```

## `@layer` — explicit cascade layers

Sometimes order matters more than specificity. `@layer` lets you say:

```css
@layer reset, tokens, base, components, utilities;

@layer reset {
  *, *::before, *::after { box-sizing: border-box; }
}

@layer tokens {
  :root { --primary: #6d28d9; }
}

@layer components {
  .card { padding: 20px; }
}

/* utility wins even if loaded before components */
@layer utilities {
  .no-padding { padding: 0 !important; }
}
```

Without `@layer`, the cascade favors the last-loaded rule. With `@layer`, utility layers always beat component layers — predictable.

Import a separate file into a layer:

```css
@layer components {
  @import "components.css" layer(components);
}
```

## Performance — what to watch

### Animate `transform` and `opacity` only

These are GPU-accelerated. Animating `width`, `height`, `top`, `left`, `margin` causes repaints and stutters.

### Avoid expensive selectors

```css
/* slow */
* { … }
div a[href*="example"] { … }

/* fast */
.specific { … }
```

### Selector length

Shorter is faster, but don't stress — modern browsers optimize well.

### `will-change` — use sparingly

```css
.card { will-change: transform; }   /* only while animating */
```

Pre-creates a layer. Don't apply to everything — uses memory.

### Reduce reflows

Group DOM changes. Read window-size once, don't read & write in a loop.

### Smaller files

CSS minifiers strip spaces. Build tools do this. Don't micro-optimize by hand.

## Common debugging tools

- **Chrome DevTools — Computed panel**: see every property and its value.
- **Chrome DevTools — Styles panel**: see all rules that apply, crossed out losers.
- **Chrome DevTools — Animations panel**: pause / scrub animations.
- **Chrome DevTools — Coverage tab**: see which CSS rules are unused.
- **Chrome DevTools — Layers panel**: see stacking contexts in 3D.
- **Firefox — Layout panel**: see flex/grid layouts visually.
- **Lighthouse**: audits performance, accessibility, SEO.

## Naming conventions

### BEM

```css
.card { … }
.card__title { … }              /* child */
.card__title--featured { … }    /* modifier */
.card--lg { … }                 /* size variant */
```

Clear at a glance. Verbose but readable.

### Utility-first

```html
<button class="bg-purple text-white px-4 py-2 rounded">Click</button>
```

Fast to write, but spreads styles across HTML.

### CSS Modules / scoped

In build tools, each component's CSS is automatically scoped:

```css
.button_hauwa_1xyz { … }
```

You don't need to know the names — the build tool generates them.

Pick a convention; consistency is more important than which one.

## Try it yourself

Take **one** of your earlier lessons and refactor it:

1. Move values into CSS variables.
2. Group rules into sections with comments.
3. If it has animated transforms, add `will-change`.

## Quick quiz

1. What's the role of `@layer`?
2. Which two properties are GPU-accelerated?
3. Why avoid `* { ... }` rules for normal styling?

<details>
<summary>Answers</summary>

1. It groups rules into layers so the cascade order is explicit, not just by source order.
2. `transform` and `opacity`.
3. It matches every element — slow.

</details>

## Mini challenge

Refactor one of your practice files to use:
- `:root` tokens for colors and radii
- `@layer reset, base, components, utilities`
- A `.btn` component class used in two sizes (`.btn--sm`, `.btn--lg`)

## What's next

Now we round out the professional polish: accessibility, support, and best practices.

→ [Lesson 49 — Accessibility, @supports & Best Practices](49-accessibility-supports.md)
