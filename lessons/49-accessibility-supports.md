# Lesson 49 — Accessibility, @supports & Best Practices

> **The curb-cut analogy.** A curb cut is a small ramp at a street corner — made for wheelchair users, but used by everyone (moms with strollers, cyclists, delivery people). Accessible CSS is the same: built for users with disabilities, but everyone benefits from better contrast, keyboard support, and predictable interactions.

## What you'll learn

- Accessibility fundamentals in CSS.
- `@supports` for feature detection.
- Hiding content accessibly.
- A short list of "best practices" for production CSS.

## Why this matters

Web accessibility is required by law in many places, and it's the right thing everywhere else. Most accessibility fixes are CSS-only.

## Accessibility — the core rules

### 1. Don't remove focus styles — make them visible

```css
:focus { outline: none; }                /* DON'T */
:focus-visible { outline: none; }        /* still bad */

/* Good */
:focus-visible {
  outline: 3px solid #6d28d9;
  outline-offset: 2px;
}
```

### 2. Ensure color contrast

- Body text: 4.5:1 minimum.
- Large text (≥18pt or 14pt bold): 3:1 minimum.

Use [webaim.org/resources/contrastchecker](https://webaim.org/resources/contrastchecker/).

### 3. Don't rely on color alone

```css
.required { color: red; }     /* bad */
.required::after { content: " *"; color: red; }   /* shape and color */
```

Add a `*`, an icon, or weight/border to convey meaning.

### 4. Tap targets

Touch targets should be at least 44×44 px CSS.

```css
.btn-sm {
  padding: 8px 12px;       /* visible small button — should still be 44×44 on touch */
}
.btn-sm { min-width: 44px; min-height: 44px; }
```

### 5. Respect motion preferences

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

### 6. Hide content accessibly

There are 4 ways to hide content. Each has a use:

```css
/* 1. Visually hidden but readable by screen readers */
.sr-only {
  position: absolute;
  width: 1px; height: 1px;
  padding: 0; margin: -1px;
  overflow: hidden;
  clip: rect(0,0,0,0);
  white-space: nowrap;
  border: 0;
}

/* 2. Hidden from everyone (incl. screen readers) */
.hidden { display: none; }

/* 3. Visible to screen readers (rare) */
.aria-only { … }

/* 4. Take out of layout visually but stay accessible */
.invisible { visibility: hidden; }
```

### 7. Don't `outline: none` without replacement

Sites sometimes kill focus rings for "looks." Replace with custom styles that still meet contrast.

### 8. Form fields need labels

```html
<label for="email">Email</label>
<input id="email" type="email">
```

## `@supports` — feature detection

Apply rules only if the browser supports a feature:

```css
@supports (display: grid) {
  .layout {
    display: grid;
    grid-template-columns: 240px 1fr;
  }
}

@supports not (display: grid) {
  /* fallback */
  .layout {
    display: flex;
  }
}

@supports (backdrop-filter: blur(10px)) {
  .panel { backdrop-filter: blur(10px); }
}
```

In modern browsers you barely need this — most modern properties are baseline. Use it for cutting-edge features like `:has()` or `@property`.

### The `selector(:has())` check

```css
@supports selector(:has(*)) {
  .card:has(img) { padding: 0; }
}
```

### A polyfill pattern

```css
:root { --has-grid: 0; }
@supports (display: grid) {
  :root { --has-grid: 1; }
}

@container style(--has-grid: 1) { /* inside modern browsers */ }
```

## Browser support — a quick mental model

Modern CSS has a "baseline" — properties 95%+ of browsers support. Once a property is baseline, you can use it freely:

- CSS Grid — baseline.
- Flexbox — baseline.
- CSS variables — baseline.
- `clamp()`, `min()`, `max()` — baseline.
- `aspect-ratio` — baseline.
- Container queries — baseline.
- `:has()` — baseline in 2023+.
- `@property` — modern.

For projects that need to support ancient browsers, use [caniuse.com](https://caniuse.com) to check.

## The professional checklist

When shipping CSS, ask:

- [ ] No "always hidden" focus rings.
- [ ] Color contrast 4.5:1+ for body text.
- [ ] Buttons and links have `:focus-visible` styles.
- [ ] Buttons are at least 44×44 px on touch.
- [ ] Images have `alt` text.
- [ ] Forms have labels.
- [ ] Animations respect `prefers-reduced-motion`.
- [ ] Layouts work on phones (test in DevTools).
- [ ] `loading="lazy"` on below-fold images.
- [ ] CSS file is minified for production.

## Try it yourself

Take any earlier practice file and run the checklist. For each missing item, fix it. Pay attention to:

1. `:focus-visible` everywhere you have interactive elements.
2. Body text contrast — try `#888` on `#fff` and see if it's readable.
3. `prefers-reduced-motion` block.
4. `alt` text on every image.

## Quick quiz

1. Write the 5 lines of CSS that hide content visually but not from screen readers.
2. What does `@supports` do?
3. What's the minimum touch target size?

<details>
<summary>Answers</summary>

1. (see `.sr-only` above)
2. Applies rules only if the browser supports a given feature.
3. 44×44 px (Apple) or 48×48 px (Google).

</details>

## Mini challenge

Take an old practice file and add the full professional checklist. Test in DevTools mobile mode and with "prefers-reduced-motion: reduce" on.

## What's next

The grand finale — a translation map from pure CSS to Tailwind, so everything you've learned converts cleanly.

→ [Lesson 50 — From Pure CSS to Tailwind (the Map)](50-to-tailwind.md)
