# Lesson 01 — What is CSS?

> **The wardrobe analogy.** HTML is the body of a mannequin. CSS is the wardrobe, the makeup, the haircut, the pose. Without CSS, every website looks like an unstyled Word document. With CSS, it looks like a magazine.

## What you'll learn

- What CSS stands for and what it does.
- Why HTML alone is not enough.
- How CSS and HTML work together.
- The 3 high-level tools CSS gives you.

## Why this matters

Every beautiful page on the internet — YouTube, your school portal, TikTok — is **HTML shaped by CSS**. If you only know HTML, you can put words on a screen. If you know HTML + CSS, you can build anything a single screen needs.

## How it works

CSS stands for **Cascading Style Sheets**. Three big words:

1. **Cascading** — rules fall down like a waterfall. Later rules can override earlier ones, but stronger rules win even if they're later.
2. **Style** — what it does. It styles things.
3. **Sheets** — the rules live in a stylesheet, a separate document (or `<style>` block).

You write small instructions like *"make every paragraph dark blue"* and the browser obeys them across the whole page.

### The 3 high-level tools CSS gives you

| Tool | Purpose | Example |
|------|---------|---------|
| **Selectors** | Choose which HTML to style | `p`, `.card`, `#main` |
| **Properties & values** | Say what to change | `color: red;` |
| **Cascading rules** | Decide who wins when rules conflict | Later + more specific wins |

The simplest CSS rule looks like this:

```css
p {
  color: tomato;
}
```

This reads: *"Find every `<p>` and paint its text tomato-orange."*

That's it. Everything you'll learn from now on is variations on this pattern.

## Try it yourself

Create `practice/lesson01.html` and paste this:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 01</title>
  <style>
    body {
      font-family: system-ui, sans-serif;
      background: #fffbf5;
      padding: 30px;
    }
    h1 {
      color: #6d28d9;
    }
    p {
      color: tomato;
      font-size: 18px;
    }
    .fancy {
      color: teal;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <h1>Hello, CSS!</h1>
  <p>I'm a normal paragraph.</p>
  <p class="fancy">I'm a fancy paragraph.</p>
</body>
</html>
```

Open the file in your browser. You should see:

- A purple heading.
- A tomato paragraph.
- A bold teal paragraph that uses a **class** called `fancy`.

Now try this: change `tomato` to `dodgerblue`. Save, refresh. The color changes. *You're a CSS developer now.*

## Common mistakes

- **Forgetting the semicolon.** `color: red` is broken. `color: red;` is correct. (Some browsers tolerate the last one, but always include it.)
- **Forgetting the curly braces.** Every rule needs `{` and `}`.
- **Putting CSS in the HTML file by accident.** This lesson uses `<style>` inside HTML for simplicity. We'll move it to a separate file in Lesson 02.

## Debugging tips

- **Nothing changed?** Make sure the file saved (`Ctrl+S` / `Cmd+S`) and that you refreshed the browser.
- **Color is wrong?** Some words aren't colors (e.g. `redish` won't work). Use a real CSS color name or `#hex`.

## Quick quiz

1. What does CSS stand for?
2. What's the difference between HTML and CSS in one sentence each?
3. Change the line `p { color: tomato; }` to `p { color: #ff6347; }`. Will the paragraph look the same, different, or broken?

<details>
<summary>Answers</summary>

1. Cascading Style Sheets.
2. HTML is the **structure** (the words and boxes). CSS is the **style** (their appearance).
3. The same color! `tomato` and `#ff6347` are identical values; `#` is just the hex form.

</details>

## Mini challenge

In `lesson01.html`, add a fourth paragraph with `class="fancy emphasis"` and a CSS rule for `.emphasis` that makes the background yellow. (We'll cover multiple classes properly in Lesson 04 — for now, just try it and see what happens.)

## What's next

We learned that CSS is a set of rules. Now let's see how to write those rules cleanly. 

→ [Lesson 02 — CSS Syntax & How to Add CSS](02-syntax.md)
