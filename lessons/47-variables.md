# Lesson 47 — CSS Variables & @property

> **The paint-can analogy.** CSS variables are paint cans in a workshop. You name them by their job ("the wall color," "the line color"), and refill them once to change every wall in every room. `@property` upgrades each can with a label so the browser knows what's inside.

## What you'll learn

- The `--name` and `var()` syntax.
- The `:root` pattern.
- Light/dark theming with variables.
- `@property` for typed, animatable variables.
- Practical: design tokens.

## Why this matters

Once you have variables, your project becomes **changeable**. Switch dark mode, change brand color, switch the whole design — by editing 5 lines.

## Defining variables

```css
:root {
  --primary:  #6d28d9;
  --accent:   #f97316;
  --ink:      #1e293b;
  --bg:       #fffbf5;
  --radius:   12px;
}
```

`:root` is the `<html>` element. Variables defined here are available everywhere.

## Using variables

```css
button {
  background: var(--primary);
  color: white;
  border-radius: var(--radius);
}
.card {
  background: var(--bg);
  color: var(--ink);
}
```

Need a fallback? `var(--primary, #6d28d9)` — falls back if `--primary` isn't defined.

## The power — themes

```css
:root {
  --bg: #fffbf5;
  --ink: #1e293b;
  --primary: #6d28d9;
}

@media (prefers-color-scheme: dark) {
  :root {
    --bg: #0f172a;
    --ink: #f1f5f9;
    --primary: #c084fc;
  }
}

body { background: var(--bg); color: var(--ink); }
```

Same component code, completely different theme.

## Local overrides

```css
.alert {
  --primary: #dc2626;   /* override locally */
}
.alert .btn {
  background: var(--primary);   /* picks up the local --primary */
}
```

This is how component libraries create "variants" without re-styling each.

## Design tokens

Production projects organize variables like this:

```css
:root {
  /* colors */
  --color-bg:        #fffbf5;
  --color-surface:   #ffffff;
  --color-ink:       #1e293b;
  --color-muted:     #6b7280;
  --color-primary:   #6d28d9;
  --color-accent:    #f97316;
  --color-success:   #059669;
  --color-danger:    #dc2626;
  --color-border:    #e5e7eb;

  /* typography */
  --font-sans:       system-ui, -apple-system, sans-serif;
  --font-mono:       "SF Mono", Menlo, monospace;
  --fs-base:         clamp(1rem, 0.95rem + 0.2vw, 1.125rem);
  --fs-1:            clamp(1.6rem, 1.4rem + 1vw, 2rem);
  --fs-2:            clamp(2rem, 1.6rem + 1.5vw, 2.5rem);

  /* spacing */
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 16px;
  --space-4: 24px;
  --space-5: 40px;

  /* radius / shadow */
  --radius-sm: 6px;
  --radius:    12px;
  --shadow:    0 4px 14px rgba(0,0,0,0.08);
  --shadow-lg: 0 12px 24px rgba(0,0,0,0.14);
}
```

Build your component CSS using these tokens.

## `@property` — typed, animatable variables

```css
@property --angle {
  syntax: "<angle>";
  inherits: false;
  initial-value: 0deg;
}

.fan {
  --angle: 0deg;
  background: linear-gradient(var(--angle), #6d28d9, #f97316);
  transition: --angle 1s;
}
.fan:hover { --angle: 90deg; }
```

`@property` makes the browser understand the type, so it can interpolate. Otherwise custom properties always change **instantly** during transitions.

### Useful syntax strings

- `<angle>` — `0deg`, `45deg`, `0.25turn`
- `<length>` — `10px`, `1rem`
- `<percentage>` — `50%`
- `<color>` — `#fff`, `red`, `rgb(...)`
- `<number>` — `0`, `1.5`
- `<integer>` — `0`, `42`

### A typed "@property" use case — animated progress

```css
@property --p {
  syntax: "<percentage>";
  inherits: false;
  initial-value: 0%;
}
.bar {
  --p: 0%;
  background: linear-gradient(to right, #6d28d9 var(--p), #ddd var(--p));
  transition: --p .6s;
}
.bar.half { --p: 50%; }
```

Click or hover the bar — the fill animates from 0% to 50%.

## Try it yourself

Create `practice/lesson47.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Lesson 47 — variables</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }

    :root {
      --bg: #fffbf5;
      --surface: white;
      --ink: #1e293b;
      --muted: #6b7280;
      --primary: #6d28d9;
      --accent: #f97316;
      --radius: 12px;
      --shadow: 0 4px 14px rgba(0,0,0,0.08);
    }

    /* Theme by data-theme attribute on <html> */
    :root[data-theme="dark"] {
      --bg: #0f172a;
      --surface: #1e1b3a;
      --ink: #f1f5f9;
      --muted: #94a3b8;
      --primary: #c084fc;
      --accent: #fb923c;
    }

    @media (prefers-color-scheme: dark) {
      :root:not([data-theme="light"]) {
        --bg: #0f172a;
        --surface: #1e1b3a;
        --ink: #f1f5f9;
        --muted: #94a3b8;
        --primary: #c084fc;
        --accent: #fb923c;
      }
    }

    body {
      font-family: system-ui, sans-serif;
      margin: 0;
      background: var(--bg);
      color: var(--ink);
      transition: background .3s, color .3s;
    }

    .container { width: min(90%, 720px); margin: 0 auto; padding: 30px 0; }

    h1 { color: var(--primary); }
    .btn {
      background: var(--primary);
      color: white;
      border: 0;
      border-radius: var(--radius);
      padding: 10px 18px;
      cursor: pointer;
      font-weight: 600;
    }
    .btn:hover { background: var(--accent); }

    .card {
      background: var(--surface);
      border-radius: var(--radius);
      padding: 20px;
      box-shadow: var(--shadow);
      margin: 16px 0;
    }

    /* @property animated progress bar */
    @property --p {
      syntax: "<percentage>";
      inherits: false;
      initial-value: 0%;
    }
    .bar {
      --p: 30%;
      width: 100%;
      height: 14px;
      background: #ddd;
      border-radius: 999px;
      overflow: hidden;
    }
    .bar::after {
      content: "";
      display: block;
      width: var(--p);
      height: 100%;
      background: var(--primary);
      transition: --p .4s;
    }
    .bar:hover::after { --p: 80%; }

    .toggle {
      background: var(--accent); color: white; border: 0; border-radius: var(--radius);
      padding: 6px 14px; cursor: pointer;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>CSS variables</h1>
    <p>All colors and radii are variables. Toggle the theme to see.</p>

    <button class="toggle" onclick="document.documentElement.dataset.theme = document.documentElement.dataset.theme === 'dark' ? '' : 'dark'">Toggle theme</button>

    <div class="card">
      <h2>Variables in action</h2>
      <p>Cards, buttons, and badges all reference <code>var(--primary)</code>, <code>var(--surface)</code>, etc.</p>
      <button class="btn">Primary</button>
      <button class="btn" style="--primary: var(--accent)">Override</button>
    </div>

    <h2>Animated progress bar (using @property)</h2>
    <div class="bar"></div>
    <p style="color: var(--muted)">Hover to watch the fill animate.</p>
  </div>
</body>
</html>
```

Toggle the theme — the entire UI swaps.

## Common mistakes

- **Defining variables deep in a component** — keep the design tokens in `:root`.
- **Forgetting fallback values** — `var(--primary, #6d28d9)`.
- **Custom properties don't animate without `@property`** — instant snap.

## Quick quiz

1. Define a primary color variable in `:root`.
2. Use it in `.btn`.
3. What's required to animate a custom property's value smoothly?

<details>
<summary>Answers</summary>

1. `:root { --primary: #6d28d9; }`
2. `.btn { background: var(--primary); }`
3. Declare it as `@property` with the right `syntax`.

</details>

## Mini challenge

Build a "design tokens" section in your stylesheet (colors, spacing, radii) and refactor a button + card to use only those tokens. Add a "danger" tone to the button and an animated progress bar.

## What's next

With variables done, we look at how to organize large stylesheets without losing our minds.

→ [Lesson 48 — Optimization & Organization](48-optimization.md)
