# Lesson 29 — Tables

> **The spreadsheet analogy.** Tables are grids for showing organized data — like a train schedule or a school grade sheet. CSS makes them attractive: striped rows, padded cells, sticky headers when scrolling, and rounded corners if you want them modern.

## What you'll learn

- The 3 HTML tags for tables: `<table>`, `<tr>`, `<td>` (and `<th>`).
- Borders, padding, alignment.
- Striped rows with `nth-child`.
- Sticky headers, hover rows, responsive tables.
- Accessibility: `caption`, scope, `aria-label`.

## Why this matters

Tables still appear in dashboards, inboxes, sports stats, financial reports. Learning to style them well is a real-world skill.

## The basic HTML

```html
<table>
  <caption>Class grades</caption>
  <thead>
    <tr>
      <th scope="col">Name</th>
      <th scope="col">Math</th>
      <th scope="col">Science</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Hauwa</th>
      <td>92</td>
      <td>88</td>
    </tr>
  </tbody>
</table>
```

- `<table>` — the whole.
- `<caption>` — accessible title (often hidden visually).
- `<thead>` / `<tbody>` — semantic groups.
- `<tr>` — table row.
- `<th>` — header cell. Use `scope="col"` for columns, `scope="row"` for rows.
- `<td>` — data cell.

## Basic CSS

```css
table {
  border-collapse: collapse;
  width: 100%;
  border-spacing: 0;       /* redundant with collapse, but for clarity */
  background: white;
}
th, td {
  padding: 12px 16px;
  text-align: left;
}
th {
  background: #ede9fe;
  border-bottom: 2px solid #6d28d9;
}
td {
  border-bottom: 1px solid #ddd;
}
```

`border-collapse: collapse` is essential — without it, cells have a tiny gap.

## Pretty patterns

### Striped rows

```css
tbody tr:nth-child(odd)  { background: #f5f3ff; }
tbody tr:nth-child(even) { background: white; }
```

### Hover row

```css
tbody tr:hover { background: #fde68a; }
```

### Rounded corners (tricky)

Tables don't accept `border-radius` directly. Modern trick:

```css
table { border-collapse: separate; border-spacing: 0; }
table th:first-child { border-top-left-radius: 12px; }
table th:last-child  { border-top-right-radius: 12px; }
table tr:last-child td:first-child { border-bottom-left-radius: 12px; }
table tr:last-child td:last-child  { border-bottom-right-radius: 12px; }
```

### Sticky headers (great for long lists)

```css
thead th {
  position: sticky;
  top: 0;
  background: #6d28d9;
  color: white;
}
```

The header stays at the top when you scroll the table.

### Responsive tables

For wide tables on small screens, the common trick:

```css
.table-wrap {
  overflow-x: auto;
}
```

```html
<div class="table-wrap">
  <table>…</table>
</div>
```

The wrapper scrolls horizontally; the table keeps its full width.

## Try it yourself

Create `practice/lesson29.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Lesson 29 — tables</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    body { font-family: system-ui, sans-serif; padding: 30px; background: #fffbf5; }

    .wrap {
      max-width: 700px;
      margin: 0 auto;
      background: white;
      border-radius: 12px;
      padding: 24px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.06);
    }

    h1 { margin-top: 0; color: #6d28d9; }

    table {
      border-collapse: separate;
      border-spacing: 0;
      width: 100%;
      overflow: hidden;
      border-radius: 12px;
    }

    thead th {
      background: #6d28d9;
      color: white;
      padding: 12px 16px;
      text-align: left;
      font-weight: 600;
    }
    thead th:first-child { border-top-left-radius: 12px; }
    thead th:last-child  { border-top-right-radius: 12px; }

    tbody td {
      padding: 12px 16px;
      border-bottom: 1px solid #ddd;
    }
    tbody tr:nth-child(odd)  { background: #f5f3ff; }
    tbody tr:nth-child(even) { background: white; }
    tbody tr:hover           { background: #fde68a; }
    tbody tr:last-child td   { border-bottom: 0; }
    tbody tr:last-child td:first-child { border-bottom-left-radius: 12px; }
    tbody tr:last-child td:last-child  { border-bottom-right-radius: 12px; }

    .pill {
      display: inline-block;
      padding: 2px 10px;
      border-radius: 999px;
      font-size: 12px;
      font-weight: 600;
    }
    .ok   { background: #d1fae5; color: #065f46; }
    .warn { background: #fef3c7; color: #92400e; }
    .bad  { background: #fee2e2; color: #991b1b; }

    /* responsive scroll */
    .scroll {
      max-width: 700px;
      margin: 30px auto 0;
      overflow-x: auto;
    }
    .scroll table { min-width: 600px; }

    caption { caption-side: bottom; color: #6b7280; font-size: 12px; padding-top: 12px; }
  </style>
</head>
<body>

  <div class="wrap">
    <h1>Class grades</h1>
    <table>
      <caption>Quarter 3 — 2026</caption>
      <thead>
        <tr>
          <th scope="col">Name</th>
          <th scope="col">Math</th>
          <th scope="col">Science</th>
          <th scope="col">English</th>
          <th scope="col">Status</th>
        </tr>
      </thead>
      <tbody>
        <tr><th scope="row">Hauwa</th> <td>92</td> <td>88</td> <td>95</td> <td><span class="pill ok">Excellent</span></td></tr>
        <tr><th scope="row">Aisha</th>  <td>78</td> <td>84</td> <td>80</td> <td><span class="pill ok">Good</span></td></tr>
        <tr><th scope="row">Bilal</th>  <td>65</td> <td>70</td> <td>72</td> <td><span class="pill warn">Improving</span></td></tr>
        <tr><th scope="row">Chinwe</th> <td>58</td> <td>62</td> <td>55</td> <td><span class="pill bad">At risk</span></td></tr>
        <tr><th scope="row">Daniel</th> <td>82</td> <td>76</td> <td>80</td> <td><span class="pill ok">Good</span></td></tr>
      </tbody>
    </table>
  </div>

  <div class="scroll" style="margin-top: 30px;">
    <p style="color:#6b7280; text-align:center">Scroll horizontally on small screens ↓</p>
    <table>
      <thead>
        <tr>
          <th>Course</th><th>Mon</th><th>Tue</th><th>Wed</th><th>Thu</th><th>Fri</th><th>Sat</th><th>Sun</th>
        </tr>
      </thead>
      <tbody>
        <tr><th>Math</th><td>Bello</td><td>—</td><td>Aisha</td><td>Bello</td><td>—</td><td>—</td><td>—</td></tr>
        <tr><th>Eng</th><td>Aisha</td><td>—</td><td>Bello</td><td>—</td><td>Aisha</td><td>—</td><td>—</td></tr>
      </tbody>
    </table>
  </div>
</body>
</html>
```

Resize the window — the second table scrolls horizontally on narrow screens.

## Common mistakes

- **Using tables for layout** — never. Use Flexbox / Grid.
- **Missing `<thead>` / `<tbody>`** — accessibility tools need them.
- **No `border-collapse`** — see those ugly gaps.
- **Wrapping a row in a `border-radius` rather than the table** — won't work without the cell-level radius dance above.

## Debugging tips

- Make the table 100% wide to see real responsive behavior; an empty table looks tiny.

## Quick quiz

1. What does `border-collapse: collapse` do and why do we need it?
2. How do you highlight every odd row?
3. How do you keep the table headers visible while scrolling?

<details>
<summary>Answers</summary>

1. Removes the gap between cell borders. Without it, cells look separate and double borders appear.
2. `tr:nth-child(odd) { ... }`
3. `position: sticky; top: 0;` on `thead th`.

</details>

## Mini challenge

Build a "timetable" table with 5 weekdays × 8 periods. Add sticky headers (days), zebra rows, and a hover effect that highlights the whole row.

## What's next

Tables done. Now the other form-data heavy element: forms.

→ [Lesson 30 — Forms (accessible)](30-forms.md)
