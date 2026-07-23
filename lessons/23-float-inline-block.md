# Lesson 23 — Float & Inline-block

> **The book-on-a-shelf analogy.** `float` was originally designed to make images "float" to the left or right of text — like a book propped sideways against a stack. Today it's mostly a historical curiosity, but you need to know it exists because you'll see it in old code and certain layouts still rely on it.

## What you'll learn

- How `float` works.
- How to "clear" floats (and why you sometimes need to).
- `display: inline-block` for simple horizontal layouts.
- Why Flexbox and Grid are the modern choices.

## Why this matters

The web has **thousands of pages** built with `float`. Even modern libraries occasionally fall back to it for text-wrapping. We'll recognize it, know its traps, and move on to Flexbox.

## Float basics

```css
img {
  float: left;
  margin-right: 16px;
}
```

The image is "pulled out" of normal flow to the **left**. Text following the image wraps around its right side.

```css
.float-left  { float: left; }
.float-right { float: right; }
```

Floats only affect **block elements that come after them** in the HTML.

## Clearing floats

The reason floats are tricky: the parent container **collapses** because the floated children aren't part of its flow.

```html
<div class="parent">
  <img class="left">
  <p>Text wrapping the image…</p>
</div>
```

Without clearing, `.parent` looks empty. Add one of these after the float:

```css
/* Option 1 — clear property */
.clearfix::after {
  content: "";
  display: block;
  clear: both;
}

/* Option 2 — overflow */
.parent { overflow: hidden; }    /* creates a new BFC */

/* Option 3 — display: flow-root (modern!) */
.parent { display: flow-root; }
```

`display: flow-root` is the **modern answer**. Use it instead of the `.clearfix` hack.

## Inline-block layouts — the 2010s pattern

Before Flexbox, designers used `display: inline-block` to put boxes in a row without float hacks.

```css
.col {
  display: inline-block;
  vertical-align: top;
  width: 30%;
  margin-right: 1%;
}
```

It works but has issues:

- Boxes are affected by **whitespace in HTML** (gaps appear unless you remove newlines or set `font-size: 0` on the parent).
- Vertical alignment is finicky.
- Heights can break.

That's why Flexbox (Lesson 38) was invented.

## Where float is still useful

- Drop caps (first letter of an article):
  ```css
  p::first-letter {
    float: left;
    font-size: 3em;
    margin: 4px 6px 0 0;
  }
  ```
- Text-wrap around images in articles (pure CSS).
- Print-style preview pages.

Most modern code reaches for **Flexbox** (Lesson 38) or **Grid** (Lesson 40) instead.

## Try it yourself

Create `practice/lesson23.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 23 — float &amp; inline-block</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body { font-family: system-ui, sans-serif; padding: 30px; background: #fffbf5; }

    .article {
      max-width: 700px;
      margin: 0 auto 30px;
      background: white;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.08);
    }
    .article img {
      float: left;
      width: 160px;
      height: 100px;
      margin: 0 16px 8px 0;
      border-radius: 8px;
      background: linear-gradient(135deg, #6d28d9, #f97316);
    }
    .clearfix::after { content: ""; display: block; clear: both; }

    .flowroot { display: flow-root; }      /* clear floats without extra markup */

    /* inline-block columns */
    .cols { font-size: 0; }                 /* kill the gap from whitespace */
    .col {
      display: inline-block;
      vertical-align: top;
      width: 32%;
      margin: 0 0.5%;
      background: #ede9fe;
      padding: 16px;
      border-radius: 8px;
      font-size: 14px;
    }

    /* drop cap demo */
    .dropcap::first-letter {
      float: left;
      font-size: 4em;
      line-height: 0.9;
      padding: 6px 8px 0 0;
      color: #6d28d9;
    }
  </style>
</head>
<body>
  <h1>Float &amp; inline-block</h1>

  <h2>Image floats left, text wraps right</h2>
  <div class="article flowroot">
    <img alt="">
    <p>Floats take an element out of normal flow and push it to one side, allowing inline content to wrap around it. The parent is given <code>display: flow-root</code> so it doesn't collapse.</p>
    <p>The article continues naturally on a new line below the image.</p>
  </div>

  <h2>Without flow-root (collapsed parent)</h2>
  <div style="background:#fef3c7; padding:8px;">
    <img alt="" class="lefted" style="float:left; width:60px; height:40px; background:#f97316; margin-right: 10px;">
    <p style="background:white; padding:8px;">Look — the yellow parent has no visible height because the float was never cleared.</p>
  </div>

  <h2>inline-block 3-column row</h2>
  <div class="cols">
    <div class="col">Column one</div><div class="col">Column two</div><div class="col">Column three</div>
  </div>
  <p style="text-align:center; color:#6b7280;">Note the lack of spaces in HTML — required with inline-block to avoid gaps.</p>

  <h2>Drop cap using float</h2>
  <div class="article flowroot">
    <p class="dropcap">A drop cap is the giant first letter at the start of a chapter. Easy with <code>float: left</code> on <code>::first-letter</code>.</p>
  </div>
</body>
</html>
```

Open it. Notice how the second yellow demo has no height until you clear the float.

## Common mistakes

- **Forgetting to clear** — parent has zero height.
- **Floating everything** — not what float is for. Use Flexbox.
- **inline-block gap from whitespace** — solved by `font-size: 0` on the parent (but remember to set a real font size on the children).

## Debugging tips

- If a parent collapses with floated children, give it `display: flow-root`. That fixes 95% of float bugs.

## Quick quiz

1. How do you make a parent contain floated children without adding empty DOM?
2. Why did designers abandon inline-block for layouts?
3. What modern CSS property clears floats?

<details>
<summary>Answers</summary>

1. Add `display: flow-root;` to the parent.
2. The whitespace-between-columns problem and the alignment issues.
3. `display: flow-root` (or `overflow: hidden`).

</details>

## Mini challenge

Make an "old-school" webpage header: logo on the left, navigation links on the right, **using only `float`** (the 2005 way). Then write the same header with Flexbox and compare.

## What's next

One more positioning concept — how do we center things?

→ [Lesson 24 — Centering & Alignment](24-centering.md)
