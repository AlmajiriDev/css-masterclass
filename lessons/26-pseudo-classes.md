# Lesson 26 — Pseudo-classes

> **The mood-ring analogy.** A pseudo-class is the **mood** of an element, not the element itself. The same `<a>` is in a different mood when hovered, when visited, when active, when it's the only one, when it's the 3rd child. CSS lets you style by mood using `:mood`.

## What you'll learn

- The most-used pseudo-classes: `:hover`, `:focus`, `:active`, `:visited`, `:disabled`, `:checked`.
- Structural pseudo-classes: `:first-child`, `:nth-child()`, `:last-child`.
- Negation: `:not()`.
- Practical recipes: zebra tables, alternating lists, hover dropdowns.

## Why this matters

Pseudo-classes turn CSS from "paint everything" into "respond to the user's actions." Without them, no hovers, no focus rings, no toggle styles.

## The classic 4 link states (LVHA — from Lesson 14)

```css
a:link    { color: blue; }
a:visited { color: purple; }
a:hover   { color: orange; }
a:active  { color: red; }
```

Order matters: link → visited → hover → active.

## State pseudo-classes — input, button, form

```css
input:focus    { outline: 2px solid #6d28d9; }
input:disabled { background: #f1f5f9; color: #888; cursor: not-allowed; }
input:checked  + label { text-decoration: line-through; }
input:required { border: 2px solid orange; }
input:invalid  { border-color: red; }
input:valid    { border-color: green; }
button:hover   { background: #6d28d9; }
button:active  { transform: translateY(1px); }    /* press effect */
```

`:focus-visible` is the modern safe choice for keyboard-only focus.

## Structural pseudo-classes

These target **position-based** elements.

```css
li:first-child     { font-weight: bold; }    /* the first child in its parent */
li:last-child      { color: gray; }
li:nth-child(odd)  { background: #f5f3ff; } /* zebra striping */
li:nth-child(even) { background: white; }
li:nth-child(3)    { color: orange; }        /* the 3rd one */
li:nth-child(3n)   { font-style: italic; }   /* every 3rd */
li:nth-child(3n+1) { /* 1st, 4th, 7th, ... */ }
li:only-child      { background: gold; }      /* the only child */
li:nth-last-child(2) { /* second from last */ }
```

### `nth-of-type` vs `nth-child`

```css
p:nth-child(2)       /* 2nd child ONLY if it's a <p> */
p:nth-of-type(2)     /* 2nd <p> regardless of others */
```

`nth-of-type` is handier when siblings are mixed.

### `nth-child(an+b)` formula

```css
li:nth-child(2n)    /* even */
li:nth-child(2n+1)  /* odd */
li:nth-child(4n+1)  /* 1st, 5th, 9th */
```

`n` runs from 0. So `2n` = `0,2,4,6...` and `2n+1` = `1,3,5,7...`

## Negation and matching

```css
li:not(.done)    { color: black; }    /* every li except .done */
a:not([href])    { color: gray; }     /* placeholder links */
*:not(body)      { ... }              /* everything except body */

:is(h1, h2, h3) { margin-bottom: 12px; }   /* any of these */
:where(h1, h2, h3) { margin-bottom: 12px; } /* same selectors, zero specificity! */
```

`:where()` is brilliant — it lets you reset default margins without breaking the cascade. Use it for utility resets.

## Try it yourself

Create `practice/lesson26.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 26 — pseudo-classes</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body {
      font-family: system-ui, sans-serif;
      background: #fffbf5;
      padding: 30px;
    }
    h1, h2 { color: #6d28d9; }

    /* LVHA links */
    a:link    { color: #6d28d9; }
    a:visited { color: #5b21b6; }
    a:hover   { color: #f97316; }
    a:active  { color: #dc2626; }

    /* button press */
    button {
      background: #6d28d9;
      color: white;
      padding: 10px 16px;
      border: 0;
      border-radius: 8px;
      cursor: pointer;
      transition: background .15s, transform .1s;
    }
    button:hover  { background: #5b21b6; }
    button:active { transform: translateY(2px); }
    button:disabled { background: #c7d2fe; cursor: not-allowed; }

    /* zebra table */
    table {
      border-collapse: collapse;
      width: 100%;
      margin: 20px 0;
    }
    table th, table td {
      border: 1px solid #ddd;
      padding: 10px 14px;
    }
    tr:nth-child(odd)  { background: #f5f3ff; }
    tr:nth-child(even) { background: white; }
    /* highlight first and last row */
    tr:first-child { background: #ede9fe; font-weight: bold; }
    tr:last-child  { background: #fef3c7; }

    /* form states */
    input[type="text"] {
      padding: 8px 10px;
      border: 2px solid #ddd;
      border-radius: 6px;
      font-size: 14px;
    }
    input:focus      { border-color: #6d28d9; outline: none; }
    input:disabled   { background: #f1f5f9; color: #aaa; }
    input:invalid    { border-color: #dc2626; }
    input:valid      { border-color: #059669; }

    /* checkbox hack */
    .todos {
      list-style: none;
      padding: 0;
      max-width: 320px;
    }
    .todos li {
      display: flex;
      align-items: center;
      gap: 10px;
      padding: 8px;
      border-radius: 6px;
    }
    .todos input:checked + label {
      color: #aaa;
      text-decoration: line-through;
    }

    /* every 3rd highlight */
    .months li:nth-child(3n) {
      background: #fde68a;
    }
    .months li:nth-child(3n+1) {
      background: #ddd6fe;
    }
    .months {
      list-style: none;
      padding: 0;
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(80px, 1fr));
      gap: 6px;
    }
    .months li {
      padding: 12px;
      border-radius: 6px;
      background: #f1f5f9;
      text-align: center;
    }
  </style>
</head>
<body>
  <h1>Pseudo-classes</h1>

  <h2>Link states</h2>
  <p>Try: <a href="#">a regular link</a>, this one is visited: <a href="https://developer.mozilla.org">MDN</a>.</p>

  <h2>Button states</h2>
  <p>
    <button>Hover &amp; press me</button>
    <button disabled>Disabled</button>
  </p>

  <h2>Form states</h2>
  <p><input type="text" placeholder="focus me"></p>
  <p><input type="email" placeholder="valid email → green" value="hi@example.com"></p>
  <p><input type="email" placeholder="invalid → red" value="not-an-email"></p>
  <p><input type="text" disabled value="disabled field"></p>

  <h2>Checkbox hack — strike-through done items</h2>
  <ul class="todos">
    <li><input type="checkbox" id="t1"><label for="t1">Buy groceries</label></li>
    <li><input type="checkbox" id="t2" checked><label for="t2">Wash the car</label></li>
    <li><input type="checkbox" id="t3"><label for="t3">Walk the dog</label></li>
  </ul>

  <h2>Zebra striping &amp; first/last highlight</h2>
  <table>
    <tr><td>Header row (first-child → purple)</td></tr>
    <tr><td>Even row</td></tr>
    <tr><td>Odd row</td></tr>
    <tr><td>Even row</td></tr>
    <tr><td>Last row (last-child → gold)</td></tr>
  </table>

  <h2>Every 3rd month pattern</h2>
  <ul class="months">
    <li>Jan</li><li>Feb</li><li>Mar</li>
    <li>Apr</li><li>May</li><li>Jun</li>
    <li>Jul</li><li>Aug</li><li>Sep</li>
    <li>Oct</li><li>Nov</li><li>Dec</li>
  </ul>
</body>
</html>
```

Open it and try:
- Hover and click links/buttons.
- Tab into form fields.
- Tick the checkbox — the label strikes through.
- Notice the table first/last rows and zebra stripes.

## Common mistakes

- **`:focus` everywhere** — replaces the focus ring for mouse users too. Use `:focus-visible` instead.
- **`nth-child` counting wrong** — remember all children are counted, not just the matching tag.
- **`a:hover` without `a:link`** — `a` styling works without setting `:link` first, but it's good practice.

## Debugging tips

- In DevTools, click the `:hov` button (top-left of the Styles panel) to toggle pseudo-classes on the selected element. Crucial for debugging hover styles.

## Quick quiz

1. Write a CSS rule that styles the **3rd** `<li>` in every list.
2. What does `:not(.x)` mean?
3. How do you highlight every 4th row starting at row 2?

<details>
<summary>Answers</summary>

1. `li:nth-child(3) { ... }`
2. "An element that doesn't have class `x`."
3. `tr:nth-child(4n+2)` (gives 2, 6, 10, …).

</details>

## Mini challenge

Build a "calendar" where the 1st day of the month is highlighted (`:first-child`), the last is bold (`:last-child`), and weekends (every 7th day) have a yellow background.

## What's next

We style by state. Now we style **parts of elements** — pseudo-elements.

→ [Lesson 27 — Pseudo-elements](27-pseudo-elements.md)
