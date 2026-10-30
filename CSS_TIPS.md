# CSS Tips for the Landing Page

These notes are a reference for writing CSS that stays understandable as the site grows. Plain, modern CSS is enough for the initial landing page; a framework is not necessary.

## 1. Start mobile-first

Write the base styles for small screens, then add complexity when more space is available.

```css
.hero {
  padding: 3rem 1rem;
}

@media (min-width: 48rem) {
  .hero {
    padding: 6rem 2rem;
  }
}
```

Choose breakpoints based on where the design becomes cramped, rather than targeting particular devices.

## 2. Use predictable box sizing

A small reset removes many surprising browser defaults.

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}

body {
  margin: 0;
}

img {
  display: block;
  max-width: 100%;
}
```

With `border-box`, padding and borders remain inside an element's declared width.

## 3. Define design tokens

Keep recurring colors, spacing, and dimensions in custom properties.

```css
:root {
  --color-background: #fafafa;
  --color-text: #202020;
  --color-accent: #4263eb;

  --space-sm: 0.5rem;
  --space-md: 1rem;
  --space-lg: 2rem;

  --content-width: 70rem;
}
```

This keeps values consistent without requiring an elaborate design system.

## 4. Constrain page content

Text becomes difficult to read when it spans an entire large monitor.

```css
.container {
  width: min(100% - 2rem, var(--content-width));
  margin-inline: auto;
}

.introduction {
  max-width: 65ch;
}
```

For paragraphs, a line length of roughly `60ch` to `70ch` is usually comfortable.

## 5. Prefer normal document flow

- Use Flexbox for one-dimensional arrangements.
- Use Grid for rows and columns.
- Use `gap` to space related elements.
- Use normal block flow for page sections.

Avoid absolute positioning for the primary layout. It often works at one screen size but breaks when text wraps or the viewport changes.

## 6. Use flexible units intentionally

- `rem`: typography, spacing, and breakpoints
- `%`: widths relative to a parent
- `ch`: readable text widths
- `fr`: Grid proportions
- `px`: thin borders and details that should remain fixed
- `clamp()`: fluid values kept within safe limits

```css
h1 {
  font-size: clamp(2.25rem, 8vw, 5rem);
}
```

Avoid fixed heights around text. Content can grow because of screen size, font settings, or translation.

## 7. Keep selectors simple

Prefer descriptive classes.

```css
.navigation-link { }
.hero-title { }
.project-card { }
```

Avoid deeply nested selectors such as:

```css
header nav ul li a span { }
```

Deep selectors create specificity problems and tightly couple CSS to the HTML structure. Avoid `!important` unless there is a clearly understood reason to use it.

## 8. Let HTML carry meaning

Use semantic elements such as `header`, `nav`, `main`, `section`, `h1`, and `footer`. CSS should control presentation rather than compensate for unclear markup.

The page should remain logically understandable when CSS is disabled.

## 9. Include interaction and accessibility states

Do not remove focus outlines without providing a visible replacement.

```css
a:focus-visible,
button:focus-visible {
  outline: 0.2rem solid var(--color-accent);
  outline-offset: 0.2rem;
}
```

Also check that:

- Text has sufficient color contrast.
- Links are distinguishable without relying only on color.
- Buttons are large enough to select comfortably.
- Hover effects have equivalent keyboard-focus states.
- The page remains usable at 200% zoom.

Respect visitors who prefer less motion:

```css
@media (prefers-reduced-motion: reduce) {
  * {
    scroll-behavior: auto;
  }
}
```

## 10. Organize styles by increasing specificity

A manageable stylesheet order is:

1. Custom properties
2. Reset and base styles
3. Typography
4. Shared layout utilities
5. Page sections
6. Components
7. Media queries

Do not split CSS into many files prematurely. One organized stylesheet is easier to learn from than many tiny files.

## Testing habit

Regularly test the page at narrow, medium, and very wide viewport sizes. Also test it with long text and increased browser zoom. Responsive CSS is less about predicting every device and more about allowing content to behave naturally.
