# Lesson 21 — CSS Position & Offsets

> **The pin-board analogy.** Default positioning is elements sitting on the page wherever they land. CSS position properties let you **pin** elements to specific places — top-left corner, always-on-screen, or following the scroll. Each kind of "pin" is a different `position` value.

## What you'll learn

- The 5 `position` values.
- `top`, `right`, `bottom`, `left` offsets.
- `static` vs `relative` vs `absolute` vs `fixed` vs `sticky`.
- The "positioned ancestor" rule.

## Why this matters

Positioning is the secret behind every modal, every tooltip, every "scroll past me and I stick" header. Once you understand position, you can place anything anywhere.

## The 5 values

### `position: static` — the default

```css
.box { position: static; }
```

No special behavior. Offsets do nothing. We mostly ignore this.

### `position: relative` — moves from its own spot

```css
.box {
  position: relative;
  top: 10px;       /* moves 10px down from where it would have been */
  left: 20px;      /* moves 20px right */
}
```

Critically: **the space the element would have taken is still reserved.** Other elements don't reflow to fill the gap.

The bigger use of `relative` is as a **positioning context for children** (see absolute below).

### `position: absolute` — relative to a positioned ancestor

```css
.parent { position: relative; }
.child  {
  position: absolute;
  top: 10px;
  right: 10px;
}
```

`absolute` takes the element **out of normal flow** (other elements don't leave space for it) and positions it relative to the **nearest ancestor that has a non-static position**.

If no such ancestor exists, it positions relative to the `<html>` root (i.e., the page itself).

### `position: fixed` — relative to the viewport

```css
.float-button {
  position: fixed;
  bottom: 20px;
  right: 20px;
}
```

Stays put when the page scrolls. Great for:

- "Back to top" buttons.
- Floating action buttons.
- Cookie banners.
- Sticky nav bars on mobile.

**Note:** `fixed` is also relative to the nearest ancestor that has a CSS `transform`, `filter`, or `will-change` property — that creates a new positioning context.

### `position: sticky` — sticks when scrolling past

```css
.section-title {
  position: sticky;
  top: 0;
  background: white;
  padding: 10px;
}
```

The element scrolls normally **until** it would cross `top: 0`. Then it sticks there. Scrolling back up unsticks it.

Used for table headers, section labels, scrolling nav. We saw a hint of this in nav bars earlier.

## The offsets

```css
.thing {
  position: absolute;  /* or fixed */
  top:    0;
  right:  0;
  bottom: 0;
  left:   0;
}
```

Each is a distance from that edge. Use any combination. For "fill the whole parent":

```css
.fill {
  position: absolute;
  inset: 0;          /* top:0; right:0; bottom:0; left:0 */
}
```

## The "positioned ancestor" trap

If you write:

```css
.modal {
  position: absolute;
  top: 0;
}
```

The modal positions relative to the nearest positioned ancestor (or the page if none). Without `position: relative` on the parent, the modal will appear in unexpected spots.

## Try it yourself

Create `practice/lesson21.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 21 — position</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body {
      font-family: system-ui, sans-serif;
      background: #fffbf5;
      margin: 0;
      padding: 30px;
    }

    .stack > * { margin-bottom: 12px; }

    /* relative demo */
    .relative {
      background: #fde68a;
      padding: 16px;
      position: relative;
      top: 10px;
      left: 10px;
      width: 200px;
    }

    /* absolute inside a positioned parent */
    .container {
      position: relative;       /* <-- the magic word */
      width: 320px;
      height: 120px;
      background: #ede9fe;
      border-radius: 8px;
      margin-top: 16px;
    }
    .badge {
      position: absolute;
      top: -10px;
      right: -10px;
      background: #f97316;
      color: white;
      padding: 4px 10px;
      border-radius: 999px;
      font-weight: 700;
    }
    .corner {
      position: absolute;
      bottom: 8px;
      left: 8px;
      background: rgba(0,0,0,0.6);
      color: white;
      padding: 4px 8px;
      border-radius: 6px;
    }

    /* fixed button */
    .fab {
      position: fixed;
      bottom: 24px;
      right: 24px;
      width: 56px;
      height: 56px;
      border-radius: 50%;
      background: #6d28d9;
      color: white;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 24px;
      box-shadow: 0 8px 20px rgba(0,0,0,0.2);
    }

    /* sticky */
    .sticky-section {
      height: 800px;       /* tall for scrolling demo */
      background: linear-gradient(180deg, #fde68a, #fbcfe8);
      position: relative;
    }
    .sticky-header {
      position: sticky;
      top: 10px;
      background: #6d28d9;
      color: white;
      padding: 14px 20px;
      margin: 0;
      border-radius: 8px;
    }

    .fake-body { height: 200vh; background: #faf5ff; padding: 30px; }
  </style>
</head>
<body>
  <h1>Position</h1>

  <h2>relative — moved from its own spot</h2>
  <div class="stack">
    <div class="relative">I am offset 10px down &amp; 10px right.</div>
  </div>

  <h2>absolute inside a positioned parent</h2>
  <div class="container">
    <div class="badge">NEW</div>
    <div class="corner">Caption</div>
    Card body.
  </div>

  <h2>fixed — visible while scrolling</h2>
  <p>Scroll down — the floating button stays put.</p>
  <div class="fake-body">Long page…</div>

  <h2>sticky — sticks while inside the parent</h2>
  <div class="sticky-section">
    <h2 class="sticky-header">I stick when scrolled past</h2>
    <p style="padding: 30px; background: white;">Scroll. I stick to top until my parent ends.</p>
  </div>

  <a class="fab" href="#top" aria-label="Scroll to top">↑</a>
</body>
</html>
```

Scroll down. Watch:
- The orange button stays in the corner (`fixed`).
- The purple header stays at the top while you scroll inside the gradient section (`sticky`).

## Common mistakes

- **`absolute` without a positioned ancestor** — appear in the wrong place.
- **`position: relative` thinking it will move the element visibly** — yes it does, but the space is reserved.
- **`fixed` inside a transformed parent** — doesn't work, gets clipped to the parent.

## Debugging tips

- DevTools → Computed → `position` field shows the actual value used.
- The "Layers" panel (Chrome 95+) shows you positioned elements visually.

## Quick quiz

1. What's the difference between `relative` and `absolute`?
2. Why must `position: relative` usually be set on the parent of an `absolute` child?
3. What property makes a "back to top" button follow the screen?

<details>
<summary>Answers</summary>

1. `relative` reserves space and offsets from its own spot. `absolute` removes from flow and positions relative to a positioned ancestor.
2. So the absolute child knows where to anchor (otherwise it floats to the page).
3. `position: fixed`.

</details>

## Mini challenge

Build a chat-like card with a small "1" badge in the top-right corner using `position: absolute`. Add a small floating "↑" button to the bottom-right of the page.

## What's next

We can place elements anywhere. Now we layer them with **z-index**.

→ [Lesson 22 — Z-index & Stacking](22-z-index.md)
