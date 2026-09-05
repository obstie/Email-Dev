# 📧 Email Development Cheat Sheet

> **Purpose:** My day-to-day reference while learning and building HTML emails.
>
> **Rule:** Understand first, then implement. Do not blindly copy code.

---

## 1. HTML Email Basics

### Basic document structure

```html
<!DOCTYPE html>
<html lang="und" dir="ltr">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">

  <title>Email title</title>

  <style>
    /* Embedded CSS goes here */
  </style>
</head>

<body>
  <!-- Email content -->
</body>

</html>
```

### Important idea

Email HTML is different from normal website HTML because email clients have different levels of HTML/CSS support.

**Goal:** Build a reliable baseline first, then progressively enhance it.

---

# 2. Inline CSS

Inline CSS goes directly on the element:

```html
<p style="margin:0; color:#333333; line-height:1.5;">
  Hello!
</p>
```

### Why use it?

Inline styles are important for broad email-client compatibility.

### Remember

**Baseline = inline styles**

Then use embedded CSS/media queries for enhancements where supported.

---

# 3. Embedded CSS

Embedded/internal CSS goes inside:

```html
<head>
  <style>
    /* CSS */
  </style>
</head>
```

Example:

```css
.content {
  line-height: 1.5;
}
```

HTML:

```html
<p class="content">Text</p>
```

### Why use it?

One rule can apply to multiple elements.

### Email rule

Keep the `<style>` block in the `<head>`.

### Best practice

Make sure the email still works without embedded CSS.

---

# 4. CSS Selectors

## Element selector

```css
p {
  color: #333333;
}
```

Targets all `<p>` elements.

### Email caution

Avoid relying heavily on bare element selectors because email clients/ESPs may add or modify elements.

---

## Class selector

```css
.cta {
  background-color: red;
}
```

HTML:

```html
<a class="cta">SHOP NOW</a>
```

A class can be reused.

---

## ID selector

```css
#header {
  color: black;
}
```

HTML:

```html
<div id="header">
```

An ID should normally be unique.

---

## Attribute selector

```css
[href^="https"] {
  /* styles */
}
```

Targets elements based on attributes.

Useful to know, but not a main technique for everyday email work.

---

# 5. Pseudo-classes

Pseudo-classes describe a state.

Common interactive examples:

```css
.cta:hover {
  background-color: crimson !important;
}

.cta:focus {
  /* focus styles */
}
```

### Email use

`:hover` is useful for CTA links.

### Important

Mobile users generally don't hover, so hover is an enhancement, not essential functionality.

---

# 6. Descendant selectors

A space means "inside":

```css
.cta a {
  /* styles */
}
```

This means:

> Select links inside `.cta`.

Example:

```html
<div class="cta">
  <a href="https://example.com">SHOP NOW</a>
</div>
```

This is useful when we want to target only links inside a specific component.

---

# 7. Other combinators

### Child

```css
.wrapper > p
```

Only direct `<p>` children.

### Adjacent sibling

```css
img + p
```

Targets the first `<p>` immediately after an image.

### General sibling

```css
img ~ p
```

Targets `<p>` elements after an image at the same level.

### Selector list

```css
h1, h2, h3 {
  font-family: Arial, sans-serif;
}
```

One rule for multiple selectors.

---

# 8. CSS Specificity

When multiple CSS rules target the same element, specificity helps determine which rule wins.

From lower to higher:

1. Element selector
2. Class / pseudo-class / attribute selector
3. ID selector
4. Inline style

Example:

```css
p {
  color: green;
}

.content {
  color: blue;
}
```

`.content` wins because a class is more specific than an element selector.

### Important email example

Inline:

```html
<a style="background-color:red;">
```

Embedded:

```css
.cta a {
  background-color: blue;
}
```

The inline style normally wins.

To override it for something like hover:

```css
.cta a:hover {
  background-color: blue !important;
}
```

### `!important`

Use it when it is genuinely needed.

**Do not put `!important` on everything.**

---

# 9. Media Queries

Media queries allow CSS to apply only when conditions are met.

Basic pattern:

```css
@media screen and (max-width: 450px) {
  .mobile {
    /* mobile styles */
  }
}
```

Meaning:

> When the screen is 450px wide or smaller, apply these styles.

### Common features

```css
max-width
min-width
```

### Breakpoints

Do not automatically choose breakpoints based on specific phone models.

Instead:

1. Resize the email.
2. Find where the design starts looking bad.
3. Add a breakpoint there.
4. Adjust the design.

---

# 10. Responsive CTA

Example:

```css
@media screen and (max-width:450px) {
  .ctawrapper a {
    display: block;
    margin: 0 1em;
  }
}
```

### Why?

On mobile, the CTA becomes easier to tap with one hand.

### Design rule

Avoid mobile hover/interaction changes that cause the button to jump.

Avoid changing things like:

- padding
- margin
- height
- width
- font-size

on hover if they cause movement/jumping.

---

# 11. Columns

Email columns are more complicated than normal web layouts because email clients have different CSS support.

### Basic concept

Desktop:

```text
┌──────────────┬──────────────┐
│   Column 1   │   Column 2   │
└──────────────┴──────────────┘
```

Mobile:

```text
┌──────────────────────────────┐
│          Column 1            │
├──────────────────────────────┤
│          Column 2            │
└──────────────────────────────┘
```

---

# 12. Column technique: DIV + Outlook ghost table

Normal email clients use DIVs:

```html
<div style="display:table;width:100%;">

  <div style="display:table-cell;width:50%;">
    Column 1
  </div>

  <div style="display:table-cell;width:50%;">
    Column 2
  </div>

</div>
```

Outlook can receive a table version using conditional comments:

```html
<!--[if true]>
<table class="mso" role="presentation" width="100%">
  <tr>
<![endif]-->

<!-- columns -->

<!--[if true]>
  </tr>
</table>
<![endif]-->
```

### Mental model

**Normal clients → DIV layout**

**Outlook → table layout**

This is a ghost-table technique.

---

# 13. Stacking columns with a media query

Give each column a class:

```html
<div class="column"
     style="display:table-cell;width:50%;">
```

Then:

```css
@media screen and (max-width:30em) {
  .column {
    display:block !important;
    width:100% !important;
  }
}
```

Meaning:

> At smaller widths, turn each column into a full-width block so they stack.

---

# 14. Inline-block columns

Another technique is:

```css
display:inline-block;
```

This allows columns to sit beside each other when there is room and flow onto another line when there isn't.

### Important problem

Whitespace between inline-block elements can contribute to their width.

For example:

```text
50% + whitespace + 50% > 100%
```

This can cause unexpected wrapping.

Possible solutions include:

- `display:table` on the wrapper
- removing whitespace between elements
- `font-size:0` on the wrapper and resetting the font size on the columns

Know these techniques; choose based on the email/build system.

---

# 15. Outlook `.mso` class

Common Outlook/ghost-table styles:

```css
.mso {
  mso-cellspacing: 0;
  padding: 0;
  font-size: medium;
  font-family: Arial, Helvetica, sans-serif;
}
```

The `.mso` class can be reused for Outlook-specific table code.

---

# 16. Preview Text / Preheader

Preview text is the text shown near the subject line in an inbox.

Hidden preview text:

```html
<div style="display:none;">
  Preview text goes here.
</div>
```

### Important

Place it near the top of the email, inside the main wrapper where `lang` and `dir` are applied.

### Spacing

Email clients can continue pulling text after the intended preview text.

Special invisible characters can be used to push unwanted content away.

Common characters from the Parcel lesson:

```html
&#847;
&#8199;
&shy;
&nbsp;
```

### Remember

Preview text is plain text.

Do not try to style it using fake Unicode letters.

---

# 17. Accessibility

### Keep semantic order in mind

Screen readers and keyboard users generally follow the HTML/code order.

If visual order differs from code order, the experience can become confusing.

### Columns

Do not change stacking order just because it looks better without considering accessibility.

---

# 18. Progressive Enhancement

This is one of the biggest concepts in email development.

### Build in this order:

```text
Reliable baseline
      ↓
Inline CSS
      ↓
Embedded CSS
      ↓
Media queries
      ↓
Hover / other enhancements
```

The email should remain usable even if an enhancement is unsupported.

---

# 19. Testing mindset

Never assume the email works because it looks good in one preview.

Test:

### Desktop
- Normal width
- Wider viewport
- Narrower viewport

### Mobile
- Small width
- Larger mobile width

### Features
- CTA
- Images
- Preview text
- Columns
- Responsive stacking
- Hover where supported

### Always ask:

> What happens if this CSS is not supported?

If the answer is "the email becomes unusable," the implementation needs reconsideration.

---

# 20. Current Beats Practice Project

The Beats email is our ongoing learning template.

We will progressively add lessons to it rather than creating a completely new email for every lesson.

Current concepts implemented/practiced:

- HTML email structure
- Viewport meta
- Outlook MSO settings
- `.mso` class
- Email wrapper
- Fluid/max-width container
- Preview text
- Preview-text spacing
- Inline CSS
- Embedded CSS
- CTA hover
- `!important`
- Media queries
- Responsive CTA
- Beginning column work

---

# 21. Reusable Email Component Pattern

When creating a component, think:

```text
HTML structure
      ↓
Inline baseline styles
      ↓
Class name
      ↓
Embedded enhancement
      ↓
Media query if needed
      ↓
Test
```

Example:

```html
<div class="component">
  <a href="https://example.com"
     style="...baseline styles...">
    CTA
  </a>
</div>
```

Then enhance:

```css
.component a:hover {
  /* enhancement */
}
```

And responsive behavior:

```css
@media screen and (max-width:450px) {
  .component a {
    /* mobile enhancement */
  }
}
```

---

# 22. Git / GitHub Quick Reference

> Keep project commands here as we learn them.

### Check Git status

```bash
git status
```

### See branches

```bash
git branch
```

### Create a branch

```bash
git switch -c feature/name
```

### Switch branches

```bash
git switch branch-name
```

### Stage changes

```bash
git add .
```

### Commit

```bash
git commit -m "Describe the change"
```

### Push a new branch

```bash
git push -u origin branch-name
```

### Push later changes

```bash
git push
```

### Pull latest changes

```bash
git pull
```

### View commit history

```bash
git log --oneline
```

### Clone a repository

```bash
git clone REPOSITORY_URL
```

---

# 23. VS Code / Terminal Quick Reference

### Open current folder in VS Code

```bash
code .
```

### Create a file

PowerShell:

```powershell
New-Item filename.html
```

### Create a folder

PowerShell:

```powershell
New-Item -ItemType Directory folder-name
```

### Move into a folder

```powershell
cd folder-name
```

### Move up one folder

```powershell
cd ..
```

### Show current location

```powershell
pwd
```

### List files

PowerShell:

```powershell
Get-ChildItem
```

Short version:

```powershell
ls
```

---

# 24. Common CSS Units

```text
px  = fixed pixels
%   = percentage of available space
em  = relative to font size
rem = relative to root font size
```

For email, always consider how the chosen unit behaves across clients.

---

# 25. Common Email Rules to Remember

### DO

- Build a reliable baseline.
- Use inline CSS for important baseline styles.
- Use embedded CSS for reusable enhancements.
- Put embedded `<style>` in `<head>`.
- Use media queries for responsive enhancements.
- Test different viewport sizes.
- Consider Outlook.
- Consider accessibility.
- Keep components organised.
- Test after every significant change.

### DON'T

- Assume browser CSS support = email CSS support.
- Put all styling only in embedded CSS.
- Overuse `!important`.
- Use bare element selectors without thinking about email-client-added elements.
- Change CTA dimensions on hover if it causes jumping.
- Change content order without considering accessibility.
- Build columns without testing mobile.
- Blindly copy code without understanding what it does.

---

# 26. Learning Rule

When adding a new Parcel lesson:

1. **Read the lesson**
2. **Explain the idea in simple language**
3. **Identify why email developers use it**
4. **Look at the existing Beats code**
5. **Decide where the new code belongs**
6. **Add one small piece**
7. **Test it**
8. **Break/change it intentionally**
9. **Fix it**
10. **Update this cheat sheet**

> **The goal is not to memorise code.**
>
> **The goal is to understand why the code exists and know when to use it.**

---

# 📝 New Lessons / Notes

Add new concepts here as the course continues.

### Lesson:
**Date:**

**What I learned:**

**Why it matters:**

**Code pattern:**

```css
/* Add example here */
```

**Email-client warning:**

**Things I need to practise:**

