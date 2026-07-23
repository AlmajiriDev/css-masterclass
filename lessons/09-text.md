# Lesson 09 — Text Styling

> **The newspaper analogy.** A good newspaper does not just have words — it has a hierarchy. Headlines are big and bold. Subtitles are smaller. Body text is calm. Captions are tiny. CSS text properties let you create that hierarchy on a webpage.

## What you'll learn

- `color`, `text-align`, `text-transform`, `text-decoration`
- `line-height`, `letter-spacing`, `word-spacing`
- `text-indent`, `white-space`, `direction`
- The new `text-wrap: balance` and `pretty`

## Why this matters

Most of any web page **is text**. If your text looks bad, the page looks bad, even with great colors. Mastering these properties makes everything you design feel professional.

## The properties in order of importance

### `color`

```css
body { color: #1e293b; }
```

Sets the text color. Always pick a shade with good **contrast** against the background — aim for 4.5:1 minimum (this is an accessibility rule we'll cover in Lesson 49).

### `text-align`

```css
h1  { text-align: center; }
p   { text-align: justify; }    /* stretched to fill the row */
```

Options: `left` (default for LTR languages), `right`, `center`, `justify`.

> **Tip:** be careful with `justify`. On narrow screens it creates wide gaps between words. Prefer `text-align: left`.

### `text-transform`

```css
h1     { text-transform: uppercase; }
.lead  { text-transform: capitalize; }
```

Changes the displayed case without changing the underlying HTML text. Useful for buttons like `Get Started`.

### `text-decoration`

```css
a         { text-decoration: underline; }
a         { text-decoration: none; }    /* remove underlines */
.old      { text-decoration: line-through; }
.spelling { text-decoration: underline wavy red; }
```

For `a`, the shorthand combines line, style, color, thickness:

```css
a { text-decoration: underline solid #6d28d9 2px; }
```

### `line-height` — the secret of readable text

```css
p { line-height: 1.6; }   /* number = multiplier of font-size */
```

- Default is around 1.2 (cramped).
- 1.5 – 1.7 is comfortable for paragraphs.
- Headlines often look good at 1.1.

A unitless number multiplies. A value with a unit fixes it. Stick to unitless.

### `letter-spacing` and `word-spacing`

```css
h2 { letter-spacing: 0.05em; }   /* 5% of font-size wider */
p  { word-spacing: 0.1em; }
```

Great for UPPERCASE headings (default letter-spacing of all-caps looks cramped).

### `text-indent`

```css
p { text-indent: 1.5em; }
```

Indents the first line of paragraphs. Be careful — often readers prefer **no indent** plus a gap between paragraphs.

### `white-space`

```css
.poem   { white-space: pre; }      /* keep all spaces and newlines */
.nowrap { white-space: nowrap; }    /* never break the line */
p       { white-space: pre-line; }  /* respect \n but collapse spaces */
```

Useful for code samples, poetry, and `<pre>` elements.

### `word-break` and `overflow-wrap`

```css
.url   { overflow-wrap: anywhere; }   /* long URLs break anywhere */
.long  { word-break: break-word; }
```

`overflow-wrap: anywhere` saves you from awkward overflow when a long word or URL has nowhere to break.

### `direction` and `unicode-bidi`

For RTL (right-to-left) languages like Arabic:

```css
.ar { direction: rtl; }
```

Don't worry about this until you build an Arabic site.

### Modern line wrapping

```css
h1 { text-wrap: balance; }        /* even lines, no huge last line */
p  { text-wrap: pretty; }         /* avoid orphans (single last word) */
```

`balance` is wonderful for headlines — it makes the line lengths equal. `pretty` avoids the lonely word at the end of a paragraph.

## Try it yourself

Create `practice/lesson09.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 09 — Text</title>
  <style>
    body {
      max-width: 700px;
      margin: 30px auto;
      padding: 20px;
      font-family: Georgia, serif;
      background: #fffbf5;
      color: #1e293b;
    }
    h1 {
      text-transform: uppercase;
      letter-spacing: 0.08em;
      text-wrap: balance;      /* balanced multi-line header */
      border-bottom: 2px solid #6d28d9;
      padding-bottom: 8px;
    }
    h2 {
      color: #6d28d9;
      margin-top: 36px;
    }
    p {
      line-height: 1.7;
      font-size: 17px;
    }
    .lead {
      font-size: 1.2rem;
      color: #555;
      font-style: italic;
    }
    .quote {
      font-size: 1.1rem;
      line-height: 1.6;
      border-left: 5px solid #f97316;
      padding-left: 16px;
      color: #444;
      text-wrap: pretty;
    }
    a { text-decoration: underline wavy #f97316; }

    code, pre {
      font-family: "SF Mono", Menlo, Consolas, monospace;
    }
    pre {
      background: #f5f3ff;
      padding: 12px;
      border-radius: 8px;
      white-space: pre-wrap;     /* respect \n but allow wrap */
      overflow-wrap: anywhere;
    }
  </style>
</head>
<body>
  <h1>A short style tour</h1>
  <p class="lead">A small piece of text styled like a magazine.</p>

  <h2>The body</h2>
  <p>Body paragraphs use a generous line-height — usually between 1.5 and 1.7. They have no indent; instead a slight gap reads more calmly on screens.</p>

  <h2>A pull quote</h2>
  <p class="quote">"Typography is the craft of endowing human language with a durable visual form." — Robert Bringhurst</p>

  <h2>A code snippet</h2>
  <pre>function greet(name) {
  return `Hello, ${name}!`;
}</pre>

  <p>Read more about <a href="#">text styles on MDN</a>.</p>
</body>
</html>
```

Open it. Notice how line-height, color, and text-decoration combine to feel "designed".

## Common mistakes

- **Forgetting `line-height`** — tight text is the #1 sign of an amateur page.
- **Center-aligned everything** — center long text and your eyes tire fast.
- **UPPERCASE without `letter-spacing`** — looks cramped.

## Debugging tips

- Install the [WhatFont Chrome extension](https://chrome.google.com/webstore/detail/whatfont/jabopobgcpjomedmebfonigpkpfjbceb) to identify fonts on any page.

## Quick quiz

1. What does `text-transform: uppercase` do to the underlying HTML text?
2. What unit-less value would you give `line-height` for comfortable body text?
3. Which property can stop an unbroken URL from overflowing?

<details>
<summary>Answers</summary>

1. It only changes **display**. The HTML is unchanged.
2. Around `1.5` – `1.7`.
3. `overflow-wrap: anywhere` (or `break-word`).

</details>

## Mini challenge

Take any website you love (a school site, your favorite blog) and recreate just the typography of the article body using only the properties we covered. Aim for 3 levels: title, lead, paragraphs.

## What's next

Text without the right **font** is plain text. Now we choose fonts.

→ [Lesson 10 — Fonts & Web Safe Fonts](10-fonts.md)
