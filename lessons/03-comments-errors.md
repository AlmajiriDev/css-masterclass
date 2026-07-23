# Lesson 03 — CSS Comments & Errors

> **The sticky-note analogy.** CSS comments are the little sticky notes you stick on a recipe card. The browser can't read them. You and your team can. They tell future-you *why* you did this.

## What you'll learn

- How to write a CSS comment.
- What browser error messages look like and how to read them.
- The 4 most common beginner errors.

## Why this matters

When something goes wrong (and it will), your **first** debugging tool is the browser console. The sooner you read those errors, the faster you fix things. Comments help you leave breadcrumbs for yourself.

## CSS comments

### Single-line comment

```css
/* This is a comment */
h1 { color: purple; }   /* inline comment */
```

### Multi-line comment

```css
/*
  Header styles
  Updated: July 2026
  Owner:  Hauwa
*/
h1 {
  color: #6d28d9;
  font-size: 2rem;
}
```

**Rule:** CSS comments **must** start with `/*` and end with `*/`. Unlike HTML, there is no `<!-- -->` style. CSS uses the same comment syntax in one line or many.

> **Python tip:** this looks just like Python comments but with `*`. JavaScript uses the same.

## HTML comments (so you can recognize them)

```html
<!-- this is an HTML comment -->
<!-- visible in source view but never on the page -->
```

> Don't put HTML comments inside `<style>` — they're not CSS comments.

## Errors: what the browser tells you

Open `lesson02.html` from last lesson and add this line at the top of your CSS:

```css
body {
  backgroun: #fffbf5;
  color: #333;
}
```

(`backgroun` is **not** a real property — it's misspelled.)

Refresh the browser. **Nothing changed.**

Open DevTools (`F12`) → Console tab. You'll see:

```
CSS: backgroun: No such property.
```

That's the browser telling you: *"I don't know what `backgroun` is."*

### The 4 errors every beginner makes

1. **Typo in a property name** — `colour` instead of `color`. Browser ignores it.
2. **Missing semicolon** at the end of a line. Usually the *next* line breaks. Always use them anyway.
3. **Unclosed curly brace.** Browser stops reading the rest of the file.
4. **Wrong selector type.** `.button` vs `button` — class vs tag.

### A magic 1-line trick

Add this at the very top of your stylesheet to expose errors in old browsers:

```css
* { outline: 1px solid red; }    /* debug only — remove later */
```

It outlines every element in red. Great for spotting layout bugs. **Remove it before publishing.**

## Try it yourself — break things on purpose

Create `practice/lesson03.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 03 — debug demo</title>
  <style>
    body { font-family: system-ui, sans-serif; padding: 30px; }
    h1   { color: #6d28d9; }

    /* INTENTIONAL ERRORS — fix them one by one */
    h2   { colour: #f97316; }              /* typo */
    p    { color: #444 line-height: 1.6; } /* missing semicolon */
    .box { background: #fef3c7            /* missing brace */
    .note { padding: 12px; }
  </style>
</head>
<body>
  <h1>Debug this page</h1>
  <h2>Each error has a hint</h2>
  <p>Fix every line below.</p>
  <div class="box"><p class="note">I'm a note.</p></div>
</body>
</html>
```

Open it. Open DevTools console. Fix all four errors one by one. Refresh after each fix.

### The fix checklist

- Line with `colour` → rename to `color`.
- Line with two properties and one missing semi → add `;` after `#444`.
- Line with `background: #fef3c7` and no `}` → add `}` after the value.
- Optional: add a semicolon to `.note { padding: 12px; }`? The last one is OK to omit, but include it always.

## Common mistakes

- **Using `//` for comments.** JavaScript uses `//`. CSS does not.
- **Leaving a comment that contains `*/`** inside it — premature end. Keep nested comments out.

## Debugging tips

- Press `Ctrl+F5` / `Cmd+Shift+R` to do a **hard refresh** that ignores cache.
- In Chrome DevTools, the **Network** tab shows you which CSS files loaded (and which failed to load).
- Use the **Styles** panel inside DevTools to disable / edit rules live. It feels like magic the first time.

## Quick quiz

1. What's the correct comment syntax in CSS?
2. True or false: an unclosed `{` makes the browser skip the rest of the stylesheet.
3. Name the 2 browser tabs you open most often when something is wrong.

<details>
<summary>Answers</summary>

1. `/* ... */`
2. True.
3. **Console** (errors) and **Elements / Styles** (inspect rules).

</details>

## Mini challenge

Take `lesson03.html` after you fixed it and add 2 well-written comments explaining the **why** behind each property in the body rule. Make one of them multi-line.

## What's next

Comments and errors are the safety gear. Now we'll start picking which HTML elements to style — selectors!

→ [Lesson 04 — CSS Selectors (the basics)](04-selectors-basics.md)
