# Instructor Guide

> How to teach with these files — pacing, sequencing, and a few habits that make a big difference.

This guide is short on purpose. The lessons are already very thorough. Your job is to be the **warm** part of the experience — the explanations in the files do the cold, factual work.

---

## Suggested pacing (self-paced, gentle)

If she studies 30 – 45 minutes per session, here is a realistic pace:

| Phase | Lessons | Sessions | Idea |
|-------|---------|----------|------|
| Foundations | 01 – 08 | 4 – 6 | Slow and steady. Quiz every lesson. |
| Visual styling | 09 – 14 | 4 – 5 | Start playing with colors & fonts. |
| Box model & layout basics | 15 – 20 | 5 – 7 | The hardest block. Repeat. |
| Positioning & flow | 21 – 25 | 3 – 4 | Make the project 1 (profile card). |
| Pseudo selectors | 26 – 28 | 2 – 3 | Light technical block. |
| Components | 29 – 33 | 4 – 6 | Each lesson is a small component. |
| Animation & transform | 34 – 37 | 3 – 4 | Fun block. Slow down and play. |
| Layout power-ups | 38 – 42 | 5 – 7 | Flexbox + Grid. The big wins. |
| Responsive design | 43 – 46 | 4 – 6 | Always test on mobile size. |
| Professional polish | 47 – 50 | 3 – 4 | Refactor her earlier projects. |
| Projects | 01 – 07 | 6 – 10 | One project per week ish. |

Total: roughly 50 sessions of focused practice.

---

## The one-sentence summary of each phase

Use these to introduce phases out loud:

1. **Foundations** — *"CSS is the wardrobe for our HTML skeleton."*
2. **Visual styling** — *"We pick the colors, fonts, and patterns."*
3. **Box model** — *"Everything in HTML is a box with padding, a border, and breathing room outside."*
4. **Positioning & flow** — *"We decide where each box sits on the page."*
5. **Pseudo selectors** — *"We style things based on their state (hover, visited, first letter, …)."*
6. **Components** — *"We assemble boxes into real things people click."*
7. **Animation** — *"We make things move smoothly."*
8. **Layout power-ups** — *"Flexbox and Grid are our magic wands for layout."*
9. **Responsive** — *"We make it work on phones too."*
10. **Professional polish** — *"We organize, name, and test before showing the world."*

---

## A simple session template (30 – 45 min)

1. **Recap (3 min).** Ask: *"what is one thing you remember from last time?"*
2. **Read together (5 – 10 min).** Open the lesson file and read headings only.
3. **Try it yourself (15 – 20 min).** She types the example. She changes one thing. She refreshes the browser.
4. **Quiz (5 min).** She answers out loud. Praise specificity, not correctness.
5. **Mini challenge (5 – 10 min).** If she likes puzzles, do it live. If not, mark as homework.

---

## What to do when she is stuck

Don't fix it for her. Walk her through this checklist **out loud**:

1. **Did you save the file?** (Half of all "it doesn't work" is here.)
2. **Did you refresh the browser?** (Cache can lie to you.)
3. **Are you opening the `.html` file directly?** (Double-click, or right-click → open in browser.)
4. **Is the selector spelled exactly like the HTML class/id?**
5. **Did you close the curly braces and the semicolon?**
6. **Did you write `.class` for a class and `#id` for an id?**
7. **Press F12 → Console tab.** Any red error? Read it slowly.

If still stuck, take a screenshot. Inspect her code line by line.

---

## Mental models that make everything easier

These are the big "aha!" moments. When they happen, the rest is just details.

- **Box model** — every element is a box with padding, border, margin.
- **Document flow** — by default, things stack from top to bottom.
- **Specificity wars** — bigger specificity wins.
- **Flexbox** — *"I want these boxes to line up and play nicely."*
- **Grid** — *"I want a 2D chessboard of boxes."*
- **Mobile first** — design for the smallest screen, then add space.

Whenever a topic is rough, return to its model.

---

## How to assess progress (without grades)

Use these checkpoints:

- [ ] She can build a navigation bar from memory (Lesson 31).
- [ ] She can build a 3-card pricing section with Flexbox (Lesson 38).
- [ ] She can build a 12-column responsive grid (Lesson 41).
- [ ] She can make a button hover animation (Lesson 35).
- [ ] She can refactor a project with CSS variables (Lesson 47).
- [ ] She finishes the portfolio project (Project 7).

If yes to all, she's ready for Tailwind.

---

## Bridging into Tailwind

Lesson 50 is the formal **translation map**. For each concept she'll see the pure-CSS version and the Tailwind class equivalent. Encourage her to:

1. Pick one of her finished projects.
2. Convert it to Tailwind using the map.
3. Compare the two. They should look the same.

That's when Tailwind stops being scary.

---

## Two things to repeat every session

- *"Type the code."*
- *"Break the code on purpose."*

If she does those two things, she will learn faster than 90% of beginners.

---

Happy teaching — the fact that you took the time to build this course for her already means she'll go far.
