# Lesson 08 — Cascade, Inheritance & Specificity

> **The courtroom analogy.** When two CSS rules want to style the same element, **the cascade decides who wins**. Three judges: specificity, source order, and !important. Plus a rule about inheritance: some properties get passed down from parent to child (like eye color in a family), others don't (like a haircut).

## What you'll learn

- How the cascade resolves competing rules.
- Specificity numbers and how to compare them.
- Which properties inherit and which don't.
- Why `!important` is a last resort.

## Why this matters

If you can predict the cascade, you can debug any styling mystery. If you can't, you'll fight CSS forever. This is **the** concept that turns beginners into confident CSS developers.

## The cascade in plain English

When two rules match the same element, here's the order the browser checks them:

1. **!important** wins (almost always, except if both have it).
2. **More specific** selector wins.
3. **Later** declaration wins if specificity is equal.
4. **Inline styles** beat stylesheet rules unless a stylesheet rule is more specific.

That's the whole algorithm. Memorize it.

### Example

```html
<p class="note">Hi</p>
```

```css
p         { color: blue; }
.note     { color: green; }    /* ← wins: more specific than just "p" */
p.note    { color: red; }      /* ← wins: more specific still */
p         { color: purple; }   /* later "p" but still loses */
```

Final color: **red**.

## Specificity — the score

Each selector adds points:

| Selector type | Specificity points |
|---------------|--------------------|
| `*`           | 0,0,0,0 |
| `p`           | 0,0,0,1 |
| `.card`       | 0,0,1,0 |
| `#main`       | 0,1,0,0 |
| inline `style="..."` | 1,0,0,0 |
| `!important` | ∞ (overrides everything) |

A practical trick: **count class-like and id-like parts**. `.foo.bar` (two classes) = 0,0,2,0. `#foo .bar` (id + class) = 0,1,1,0. The larger tuple wins.

### Same specificity? Later wins.

```css
.box { color: blue; }
.box { color: green; }   /* this wins — same specificity, later */
```

### Inheritance — properties that pass down

Some properties, when set on a parent, also affect children.

**Inherited:**
- `color`
- `font-family`, `font-size`, `font-weight`, `line-height`
- `text-align`, `text-transform`, `letter-spacing`
- `list-style`
- `cursor`

**Not inherited (default behavior):**
- `margin`, `padding`, `border`
- `width`, `height`
- `background`, `background-image`
- `display`, `position`, `z-index`

You can force any property to inherit with `inherit`:

```css
.child { color: inherit; }   /* use parent's color even if child says otherwise */
```

Or apply the value to every element:

```css
button { color: var(--ink); }   /* not inherited by default */
```

### Use the universal selector to set inheritance-friendly defaults

```css
* {
  box-sizing: border-box;
}
button, input {
  font: inherit;       /* inputs don't inherit font by default */
}
```

### `!important` — the nuclear option

```css
.btn { color: white !important; }
```

`!important` makes a declaration beat any other rule, including inline styles. Only use it for:

- Accessibility fixes (focus outlines must be visible).
- Overriding a third-party library you cannot edit.

**If you use `!important` to win against your own code, your stylesheet is broken**. Restructure.

## Try it yourself

Create `practice/lesson08.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 08 — cascade</title>
  <style>
    body  { font-family: system-ui, sans-serif; padding: 30px; }
    p     { color: blue; }              /* specificity 0,0,0,1 */
    .note { color: green; }             /* specificity 0,0,1,0 — wins */
    p.note { color: red; }              /* specificity 0,0,1,1 — wins above */

    /* inheritance demo */
    .parent {
      color: #6d28d9;
      font-weight: 600;
      border: 1px dashed #6d28d9;
      padding: 12px;
      border-radius: 8px;
      margin-bottom: 16px;
    }

    /* child wants to override */
    .parent .bold { color: black; }

    /* this whole list inherits color & font-weight */
  </style>
</head>
<body>
  <h1>Watch the cascade!</h1>
  <p class="note">Red wins (p.note is more specific than .note).</p>
  <p>A plain paragraph is blue.</p>

  <h2>Inheritance</h2>
  <div class="parent">
    Parent text is purple and bold.
    <p>Inside a <code>&lt;p&gt;</code> — text stays purple (inherited).</p>
    <p>The border and padding are NOT inherited.</p>
    <p class="bold">I overrode the color with .bold (child class).</p>
  </div>
</body>
</html>
```

Now open DevTools, click the **first paragraph**, and read the **Styles** panel from top to bottom. The browser shows you exactly which rules won.

## Common mistakes

- **Trying to win with another `.foo`** — same specificity, just moves order.
- **Trying to override an ID with a class** — can't. IDs always win ties unless the ID rule doesn't exist.
- **Confusing `inherit` with `initial`** — `initial` resets to the spec default; `inherit` copies from parent.

## Debugging tips

- In DevTools → Styles panel, crossed-out rules show you which ones **lost** the cascade fight. Read them — they tell you what was almost applied.

## Quick quiz

1. Order these by specificity (low → high): `p`, `.x`, `#x`, `p.x`, `body #x .x`.
2. If two rules have equal specificity, which one wins?
3. Is `font-family` inherited by default?

<details>
<summary>Answers</summary>

1. `p` (1), `.x` (10), `p.x` (11), `#x` (100), `body #x .x` (110).
2. The one declared **later** in the stylesheet.
3. Yes.

</details>

## Mini challenge

Write a stylesheet with 4 rules that all style `<p class="a">` with different colors and then add a `!important` rule to see it win. Predict the result **before** refreshing — then refresh.

## What's next

We can pick colors and we know who wins. Now we apply all this to text — typography is half the work of any web page.

→ [Lesson 09 — Text Styling](09-text.md)
