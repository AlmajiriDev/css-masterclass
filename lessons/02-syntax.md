# Lesson 02 — CSS Syntax & How to Add CSS

> **The recipe analogy.** A CSS rule is a recipe card. The **selector** names the dish (for whom?), and the **declarations** are the instructions inside (what to do?). Each declaration is "property: value;" — exactly two pieces of info, separated by a colon and ending with a semicolon.

## What you'll learn

- The exact anatomy of a CSS rule.
- The 3 ways to add CSS to an HTML page.
- Which way you should use most of the time.
- How a separate stylesheet file works.

## Why this matters

If you can read a CSS rule out loud, you can write it. If you can write one, you can style any page on Earth. The structure never changes.

## The anatomy of a CSS rule

```css
selector {
  property: value;
  property: value;
}
```

Real example, annotated:

```css
button {                 /* selector = which element */
  background: purple;    /* property: value;  (background-color) */
  color: white;          /* property: value;  (text color) */
  padding: 10px 20px;    /* two values — top/bottom, left/right */
  border-radius: 8px;    /* rounded corners */
}
```

Reading rule: *"Every `<button>` has a purple background, white text, padding of 10px top/bottom and 20px left/right, and rounded corners of 8 pixels."*

**Reading tip:** Say "every" + selector, then describe each property in order. If you can read the rule, you can write it.

## The 3 ways to add CSS

### 1. External stylesheet (the recommended way)

You put all your CSS into a file, usually `style.css`, and link it inside the HTML `<head>`.

```html
<!-- index.html -->
<head>
  <link rel="stylesheet" href="style.css">
</head>
```

```css
/* style.css */
body {
  font-family: sans-serif;
}
h1 {
  color: #6d28d9;
}
```

**Why this is best:** one stylesheet can style hundreds of HTML pages.

### 2. Internal stylesheet (inside `<style>` in `<head>`)

```html
<head>
  <style>
    h1 { color: purple; }
  </style>
</head>
```

**When to use it:** small single-page projects, demos, and exercises (like in this course). Avoid it for real websites.

### 3. Inline styles (on the element itself)

```html
<h1 style="color:purple">Hi</h1>
```

**When to use it:** rarely. Hard to maintain. Each rule should always live in a stylesheet when possible.

## Try it yourself — external stylesheet edition

In your `practice/` folder, create **two files**:

**`practice/lesson02.html`**

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 02</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <h1>Big Purple Title</h1>
  <p>One short paragraph.</p>
  <button>Click me</button>
</body>
</html>
```

**`practice/style.css`**

```css
body {
  background: #fffbf5;
  font-family: system-ui, sans-serif;
  padding: 30px;
}
h1 {
  color: #6d28d9;
}
p {
  color: #444;
  line-height: 1.6;
}
button {
  background: #f97316;
  color: white;
  border: 0;
  border-radius: 8px;
  padding: 12px 24px;
  font-size: 16px;
}
```

Open `lesson02.html` in your browser. The two files are now linked. Change the color in `style.css`, refresh the browser — it updates.

> **Tip:** keep `style.css` open in VS Code on one side, and your browser on the other side. Save → refresh. Repeat forever.

## Common mistakes

- **Wrong file path.** If `style.css` is in the same folder as the HTML, `href="style.css"` works. Otherwise you need the right relative path like `../style.css`.
- **`<link>` placed outside `<head>`.** Always put it in `<head>`.
- **Forgetting `rel="stylesheet"`.** Without it the link is just decoration — the browser won't apply it.

## Debugging tips

- Open the browser's developer tools (`F12` or right-click → Inspect). The **Elements** tab shows you which rules apply to a selected element.
- The **Network** tab will tell you if `style.css` failed to load (red row).
- Use `view-source:` in the address bar to see what the browser is actually reading.

## Quick quiz

1. What are the 3 ways to add CSS to a page?
2. In the rule `p { color: red; }`, which is the **selector** and which is the **property**?
3. Where in the HTML do external stylesheets go?

<details>
<summary>Answers</summary>

1. External stylesheet, internal `<style>`, inline `style="..."`.
2. Selector: `p`. Property: `color`.
3. Inside the `<head>` section.

</details>

## Mini challenge

Add a fourth element to `lesson02.html` — an `<a href="#">link</a>` — and write a CSS rule for `a` that styles it like a button: orange background, white text, padding, and rounded corners. Hint: an `<a>` by default is **inline** and has no padding — we'll see in Lesson 09 why, but for now try `display: inline-block;`.

## What's next

Now we can write rules and link them. Next we look at the comments you leave for your future self, and the error messages browsers give you when things go wrong.

→ [Lesson 03 — CSS Comments & Errors](03-comments-errors.md)
