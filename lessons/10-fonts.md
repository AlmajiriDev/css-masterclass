# Lesson 10 — Fonts & Web Safe Fonts

> **The clothes analogy.** HTML decides what you say. CSS (fonts) decides how your words are **dressed**. A business letter in a Tailored font feels serious. The same letter in Comic Sans feels wrong. Font choice carries a mood without saying a word.

## What you'll learn

- The 5 font families in CSS (`serif`, `sans-serif`, `monospace`, `cursive`, `fantasy`).
- How to load web fonts like Google Fonts.
- `font-*` properties and the `font` shorthand.
- Web safe fallback fonts.

## Why this matters

The right font can make a generic page feel premium. The wrong font can ruin the most beautiful colors. You'll also learn why some websites take a moment to load a font.

## The 5 generic families

```css
font-family: serif;        /* with little feet */
font-family: sans-serif;   /* without feet */
font-family: monospace;    /* equal-width */
font-family: cursive;      /* handwriting-ish */
font-family: fantasy;      /* playful, decorative */
```

Browsers pick a default font for each family. We override with specific names.

## Specific fonts

Pick a real font, then list **fallbacks** in case it's missing:

```css
font-family: "Helvetica Neue", Arial, sans-serif;
```

The browser tries them in order. The last one should be a generic family — it acts as the safety net.

### Common web-safe fonts

A web-safe font is one that ships with most operating systems. If you stick to these, your site looks identical everywhere.

Sans-serif: `Arial`, `Helvetica`, `Verdana`, `Tahoma`, `Trebuchet MS`.

Serif: `Times New Roman`, `Georgia`, `Palatino`.

Monospace: `Courier New`, `Consolas`, `Monaco`.

System fonts (great for native feel):

```css
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
```

`system-ui` and even more concisely:

```css
font-family: system-ui, sans-serif;
```

This uses the OS default font — clean, fast, and matches the user's device.

## Web fonts (Google Fonts etc.)

Web fonts are fonts the browser downloads. You can load free ones from Google Fonts:

```html
<head>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap" rel="stylesheet">
</head>
```

Then use them in CSS:

```css
body { font-family: "Inter", sans-serif; }
```

Choose one or two fonts. More is visual noise.

## The `font` shorthand

```css
font: italic bold 1.2rem/1.5 "Helvetica Neue", sans-serif;
```

Order:

```
font-style font-variant font-weight font-size/line-height font-family
```

You can leave off the optional parts. Two **required**: `font-size` and `font-family`.

## Other font properties

```css
h1 { font-weight: 700; }            /* 100–900, normal=400, bold=700 */
p  { font-style: italic; }
.mono { font-variant-numeric: tabular-nums; }
.text { font-stretch: condensed; }   /* rarely used */
```

`font-weight` of variable fonts (like Inter loaded above) accepts `400`, `600` — pick from what you loaded, otherwise it falls back.

## Variable fonts (advanced note)

Some fonts support **variable weights** — instead of loading Regular and Bold separately, you load one file and request any weight 100–900. This saves bandwidth and gives you fine control.

## Try it yourself — load a real Google Font

Create `practice/lesson10.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 10</title>

  <!-- Web fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&family=Playfair+Display:ital,wght@0,600;1,600&display=swap" rel="stylesheet">

  <style>
    body {
      font-family: "Inter", system-ui, sans-serif;
      max-width: 700px;
      margin: 30px auto;
      padding: 20px;
      background: #fffbf5;
      color: #1e293b;
    }
    h1 {
      font-family: "Playfair Display", Georgia, serif;
      font-weight: 600;
      font-size: 2.5rem;
      line-height: 1.2;
      letter-spacing: -0.02em;
      margin-bottom: 0;
    }
    h2 { font-family: "Playfair Display", serif; font-weight: 600; }
    p  { font-size: 18px; line-height: 1.7; }
    .kicker {
      font-family: "Inter", sans-serif;
      text-transform: uppercase;
      letter-spacing: 0.2em;
      font-weight: 600;
      font-size: 12px;
      color: #f97316;
    }
    .credit { font-style: italic; color: #555; }
  </style>
</head>
<body>
  <p class="kicker">Style Notes</p>
  <h1>The quiet power of typography</h1>
  <p class="credit">By Hauwa • 5 min read</p>

  <h2>A second heading</h2>
  <p>Playfair Display gives the headlines a literary feel. Inter keeps the body calm. Together they make a page feel designed rather than thrown together.</p>

  <p>Resize the page and notice how the line-height and letter-spacing keep things readable at every size.</p>
</body>
</html>
```

Open it. Notice how the heading and body have clearly different voices. That's the work of font choice.

## Common mistakes

- **Too many fonts** — 1 for headings + 1 for body is plenty. 3+ looks like a mood board, not a website.
- **Loading only one weight** — if you say `font-weight: 700` but only loaded 400, the browser fakes bold (looks bad).
- **Loading multiple Google Fonts blocking render** — add `&display=swap` to show fallback text instantly while the font loads.
- **Tight letter spacing on big headers** — large text needs a tiny `-letter-spacing` (about `-0.02em`).

## Debugging tips

- In DevTools → Network filter **font**, you'll see whether web fonts loaded. If a font fails to load, you'll see **Times New Roman** suddenly appear — that's your fallback.

## Quick quiz

1. What is a web-safe font?
2. Why do we list fallbacks like `sans-serif` at the end of `font-family`?
3. What does `display=swap` do in a Google Fonts URL?

<details>
<summary>Answers</summary>

1. A font installed on most operating systems (Arial, Times New Roman, etc.).
2. So the browser has something safe to render if the chosen font is missing.
3. It shows fallback text immediately and swaps in the web font when ready (avoids invisible text).

</details>

## Mini challenge

Replace the Google Fonts in `lesson10.html` with two fonts of your choice. Then design a small "magazine cover" with:
- a kicker label
- a large headline
- a 1-paragraph subtitle
- a byline

## What's next

Text looks great. Next we dress the canvas — backgrounds!

→ [Lesson 11 — Backgrounds](11-backgrounds.md)
