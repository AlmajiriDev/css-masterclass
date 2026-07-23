# Lesson 30 — Forms (Accessible)

> **The interview-form analogy.** A form is an interview with the user. We ask questions; they answer. CSS makes the questions look friendly and the answers easy to give. Accessibility makes the interview fair for everyone — keyboard, screen reader, touch, mouse.

## What you'll learn

- Styling `<input>`, `<select>`, `<textarea>`, `<label>`, `<button>`.
- The `:focus-visible`, `:valid`, `:invalid`, `:checked` pseudo-classes for forms.
- Layout for forms (Grid is great).
- Accessibility tips: labels, focus, error messages.

## Why this matters

Forms are the input mechanism for everything — signups, login, search, checkout. A confusing form loses users. A clear, well-styled form converts.

## Reset form fields

Default inputs look totally different across browsers. Reset them first:

```css
input, select, textarea, button {
  font: inherit;          /* use page font */
  color: inherit;
}
```

## The 4 most-styled elements

```css
input[type="text"], input[type="email"], input[type="password"], textarea {
  display: block;
  width: 100%;
  padding: 10px 14px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background: white;
  font-size: 16px;
}
input[type="text"]:focus {
  outline: 3px solid #fde68a;
  outline-offset: 0;
  border-color: #6d28d9;
}
```

> **Tip:** never set `<input>` font-size below 16px on mobile — it makes the browser zoom in iOS.

## Labels — never skip them

Every input needs a `<label>`.

```html
<label for="email">Email</label>
<input id="email" type="email" name="email">
```

The `for` attribute matches the input's `id`. Screen readers read the label when the input is focused. Clicking the label focuses the input. **Both** are essential UX.

## Layout — use Grid

```css
form {
  display: grid;
  gap: 16px;
  max-width: 400px;
}
label {
  display: block;
  font-weight: 600;
  margin-bottom: 6px;
}
```

For inline checkboxes:

```css
.checkbox {
  display: flex;
  align-items: center;
  gap: 10px;
}
.checkbox input { width: 18px; height: 18px; accent-color: #6d28d9; }
```

`accent-color` is the modern way to color native checkboxes, radios, and progress bars.

## Validation states

```css
input:invalid { border-color: #dc2626; }
input:valid   { border-color: #059669; }
input:required { border-left: 4px solid orange; }

input:focus:invalid { /* alert only when focused */ }
```

To avoid showing red before the user has typed:

```css
input:not(:placeholder-shown):invalid { border-color: #dc2626; }
```

This only triggers after they've typed something.

## Accessibility checklist

- Every input has a `<label>`.
- Use `<button type="submit">` not `<div onClick>`.
- Error messages have `aria-describedby` pointing from the input.
- The first focusable element is auto-focused (but only in special forms).
- Tab order is logical (HTML order — don't fight it).
- Color is not the only indicator (also an icon or text).

## Try it yourself

Create `practice/lesson30.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 30 — forms</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body {
      font-family: system-ui, sans-serif;
      background: #fffbf5;
      padding: 30px;
      display: flex;
      justify-content: center;
    }
    form {
      display: grid;
      gap: 16px;
      width: 100%;
      max-width: 440px;
      background: white;
      padding: 28px;
      border-radius: 16px;
      box-shadow: 0 6px 24px rgba(0,0,0,0.08);
    }
    h1 { margin: 0 0 8px; color: #6d28d9; }
    label { display: block; font-weight: 600; margin-bottom: 6px; font-size: 14px; }
    .field small { color: #6b7280; font-size: 12px; }

    input, select, textarea {
      font: inherit;
      color: inherit;
      display: block;
      width: 100%;
      padding: 10px 14px;
      border: 1px solid #d1d5db;
      border-radius: 8px;
      background: white;
      font-size: 16px;
    }
    textarea { min-height: 100px; resize: vertical; }
    select   { padding-right: 36px; }

    input:focus-visible, select:focus-visible, textarea:focus-visible {
      outline: 3px solid #fde68a;
      outline-offset: 0;
      border-color: #6d28d9;
    }
    input:not(:placeholder-shown):invalid { border-color: #dc2626; }
    input:not(:placeholder-shown):valid   { border-color: #059669; }

    .row { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }

    .check { display: flex; align-items: center; gap: 10px; }
    .check input { width: 18px; height: 18px; accent-color: #6d28d9; margin: 0; }

    button {
      background: #6d28d9;
      color: white;
      padding: 12px 24px;
      border: 0;
      border-radius: 8px;
      font-weight: 600;
      cursor: pointer;
      transition: background .2s, transform .1s;
    }
    button:hover  { background: #5b21b6; }
    button:active { transform: translateY(1px); }
    button:focus-visible { outline: 3px solid #fde68a; outline-offset: 2px; }
  </style>
</head>
<body>
  <form novalidate>
    <h1>Sign up</h1>

    <div class="row">
      <div>
        <label for="fn">First name</label>
        <input id="fn" required placeholder="Hauwa">
      </div>
      <div>
        <label for="ln">Last name</label>
        <input id="ln" required placeholder="Bello">
      </div>
    </div>

    <div>
      <label for="email">Email</label>
      <input id="email" type="email" required placeholder="hello@example.com">
    </div>

    <div>
      <label for="role">Role</label>
      <select id="role">
        <option>Student</option>
        <option>Teacher</option>
        <option>Other</option>
      </select>
    </div>

    <div>
      <label for="msg">Message</label>
      <textarea id="msg" placeholder="Tell us about yourself…"></textarea>
    </div>

    <label class="check">
      <input type="checkbox" required>
      I agree to the terms
    </label>

    <button type="submit">Create account</button>
  </form>
</body>
</html>
```

Try:
- Tab through every field — see `:focus-visible`.
- Type an invalid email — see `:invalid` red border (only after typing thanks to `:not(:placeholder-shown)`).
- Type a valid email — see `:valid` green.

## Common mistakes

- **`<input>` without `<label>`** — accessibility failure.
- **`<div onClick>` instead of `<button>`** — no keyboard support, no focus order.
- **Color-only validation feedback** — colorblind users miss it.
- **Inputs too small** — keep them at least 16px font and tall tap targets (44×44 px).

## Quick quiz

1. Why does every input need a `<label>`?
2. How do you color a native checkbox purple without rebuilding it?
3. What does `accent-color` color?

<details>
<summary>Answers</summary>

1. Accessibility: screen readers need it, and clicking it focuses the field.
2. Use `accent-color: purple;` on the input or its container.
3. Native form controls: checkboxes, radios, range, progress.

</details>

## Mini challenge

Build a "contact us" form with: name, email, topic (select), message (textarea), a checkbox for "subscribe to newsletter", and a submit button. Style and validate it accessibly.

## What's next

Forms done. Now we wire forms into navigation — nav bars.

→ [Lesson 31 — Navigation Bars](31-navigation.md)
