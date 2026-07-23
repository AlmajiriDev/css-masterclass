# CSS Cheatsheet

> One page of pure-CSS essentials. Keep this open while you work.

---

## 1. The 3 ways to add CSS

```html
<!-- External (best, almost always use this) -->
<link rel="stylesheet" href="style.css">

<!-- Internal (good for single pages) -->
<style>p { color: red; }</style>

<!-- Inline (avoid when possible) -->
<p style="color:red;">Hi</p>
```

## 2. The CSS rule

```css
selector {          /* who? */
  property: value;  /* what? */
  property: value;
}
```

## 3. Selectors

```css
*               /* everything */
h1              /* every h1 */
.box            /* every element with class="box" */
#main           /* the element with id="main" */
a:hover         /* only when hovered */
input:focus     /* only when clicked into */
ul > li         /* direct child */
h1 + p          /* immediate sibling */
li:nth-child(2) /* the 2nd child */
::before        /* pseudo-element before content */
```

## 4. Colors

```css
color: red;
color: #ff0000;     /* 6-digit hex */
color: #f00;        /* 3-digit hex */
color: rgb(255,0,0);
color: rgba(255,0,0,0.5);    /* with transparency */
color: hsl(0,100%,50%);
color: hsla(0,100%,50%,0.5);
```

## 5. Units

| Unit | Used for | Example |
|------|----------|---------|
| `px`  | pixel (fixed) | `16px` |
| `em`  | relative to font-size of parent | `1.5em` |
| `rem` | relative to root font-size | `1.5rem` |
| `%`   | percent of parent | `50%` |
| `vh` / `vw` | 1% of viewport height / width | `100vh` |
| `fr`  | fraction of available space (Grid) | `1fr` |

## 6. The box model

```css
.box {
  width: 200px;
  height: 100px;
  padding: 20px;          /* space INSIDE */
  margin: 20px;           /* space OUTSIDE */
  border: 2px solid red;  /* the line in the middle */
  box-sizing: border-box; /* include padding/border in width */
}
```

## 7. Display

```css
display: block;        /* takes full width, stacks */
display: inline;       /* only as wide as content, no width */
display: inline-block; /* inline but accepts width/height */
display: none;         /* gone, no space */
display: flex;         /* flexbox */
display: grid;         /* grid */
```

## 8. Position

```css
position: static;        /* default */
position: relative;      /* shifts from its own spot */
position: absolute;      /* relative to nearest positioned parent */
position: fixed;         /* relative to viewport, doesn't move */
position: sticky;        /* sticks when scrolling past */
top: 10px; right: 20px; bottom: 10px; left: 20px;
```

## 9. Flexbox (one-liner version)

```css
.parent {
  display: flex;
  justify-content: center;       /* horizontal alignment */
  align-items: center;           /* vertical alignment */
  gap: 16px;                     /* space between kids */
  flex-direction: row;           /* or: column */
  flex-wrap: wrap;               /* allow new lines */
}
.child {
  flex: 1;                       /* take equal share */
  flex-grow: 1;
  flex-shrink: 1;
  flex-basis: 200px;
  align-self: flex-end;          /* override parent's align-items */
}
```

## 10. Grid (one-liner version)

```css
.parent {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;        /* 3 equal columns */
  grid-template-columns: repeat(3, 1fr);
  grid-template-columns: 200px 1fr;
  gap: 16px;
}
.child {
  grid-column: 1 / 3;                       /* span 2 columns */
  grid-row: 1;
}
```

## 11. Responsive

```css
@media (max-width: 600px) { /* phone size */
  .grid { grid-template-columns: 1fr; }
}

/* mobile-first helper */
@media (min-width: 600px)  { /* tablet up */ }
@media (min-width: 1024px) { /* desktop up */ }
```

## 12. Animation

```css
.btn {
  transition: all 0.2s ease;
}
.btn:hover { transform: scale(1.05); }

@keyframes bounce {
  0%   { transform: translateY(0); }
  50%  { transform: translateY(-10px); }
  100% { transform: translateY(0); }
}
.box { animation: bounce 1s infinite; }
```

## 13. Variables

```css
:root {
  --primary: #6d28d9;
  --radius: 12px;
}
.btn {
  background: var(--primary);
  border-radius: var(--radius);
}
```

## 14. Modern helpers

```css
font-size: clamp(1rem, 2vw, 2rem);    /* fluid text */
width: min(90%, 1200px);              /* pick the smaller */
gap: max(8px, 1vw);                   /* pick the larger */
height: calc(100vh - 80px);           /* math */
```

## 15. Pseudo-classes quick list

`hover`, `focus`, `active`, `visited`, `link`, `disabled`, `checked`, `first-child`, `last-child`, `nth-child(n)`, `not()`, `is()`, `where()`.

## 16. Pseudo-elements

```css
.box::before { content: "→ "; }
.box::after  { content: " ←"; }
.box::first-letter { font-size: 3em; }
.box::placeholder  { color: gray; }
```

## 17. Common properties

```
color, background, background-color, background-image, background-size,
font-family, font-size, font-weight, font-style, line-height, letter-spacing,
text-align, text-transform, text-decoration, text-shadow,
border, border-radius, box-shadow,
margin, padding, width, height, max-width, min-height,
display, position, top, right, bottom, left, z-index,
overflow, opacity, visibility, cursor,
transform, transition, animation,
flex, grid, gap, justify-content, align-items, align-content
```

## 18. Specificity (largest wins)

```
!important       → infinity
style="..."       → 1000
#id               → 100
.class, [attr], :pseudo → 10
tag, ::pseudo     → 1
*                 → 0
```

When two rules tie, **the later one wins**.

---

That's the whole language in 18 boxes. Print this page if it helps.
