# top 50 html and css ques with ans got senior developer in md format

Senior frontend developers face HTML/CSS questions testing semantic markup, modern layout systems, accessibility, performance, and browser quirks. This curated list of 50 top questions (2025) draws from GreatFrontend, InterviewBit, FrontendInterviewHandbook, and senior-level sources, focusing on practical senior scenarios like responsive systems and CSS architecture.[^1][^2][^3][^4]

## HTML Questions (1-25)

**1. Difference between HTML and HTML5?**

```
HTML5 adds semantic elements (`<article>`, `<section>`), native media (`<video>`), forms (`email`, `date`), Canvas/SVG, and APIs like localStorage vs HTML4's div-heavy, plugin-reliant approach.[^5][^6]
```

**2. What are semantic HTML elements? Why use them?**

```
`<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<aside>`, `<footer>` convey meaning to browsers/screen readers, improving SEO/accessibility over `<div>`. Ex: `<article>` for blog posts.[^7][^5]
```

**3. Block vs Inline elements?**

```
Block (`<div>`, `<p>`) take full width, start new line. Inline (`<span>`, `<a>`) flow horizontally, respect content width. `display` property overrides.[^8][^5]
```

**4. New HTML5 input types?**
`email`, `url`, `tel`, `number`, `date`, `color`, `range`, `search` provide validation/UI hints. Ex: `<input type="email">` shows @ keyboard on mobile.[^6][^5]

**5. What is `data-*` attribute?**
Custom data storage: `<div data-id="123" data-role="admin">`. Access via `dataset.id`. Useful for JS frameworks like React keys.[^3][^7]

**6. Canvas vs SVG?**
Canvas: raster, pixel-based, imperative JS drawing (games/charts). SVG: vector, declarative XML (icons/logos), scalable, DOM-manipulable.[^9][^3]

**7. Doctype declaration?**
`<!DOCTYPE html>` triggers standards mode. Without it, browsers use quirks mode with inconsistent rendering.[^6][^9]

**8. Meta viewport tag?**
`<meta name="viewport" content="width=device-width, initial-scale=1">` enables responsive design by setting viewport to device width.[^7]

**9. Figure/Figcaption use?**

```
`<figure><img src="chart.png"><figcaption>Sales Q4</figcaption></figure>` groups media + description, improves semantics/accessibility.[^10]
```

**10. How to embed responsive video?**

```html
<video controls width="100%" poster="thumbnail.jpg">
  <source src="video.mp4" type="video/mp4">
</video>
```

CSS: `aspect-ratio: 16/9` or padding hack.[^5]

**11. What is Progressive Enhancement?**
Build core functionality with HTML/CSS, enhance with JS. Ensures accessibility on low-end devices vs graceful degradation.[^9]

**12. Srcset for responsive images?**
`<img srcset="small.jpg 480w, large.jpg 1080w" sizes="(max-width: 600px) 480px, 1080px" src="fallback.jpg">` serves optimal image by viewport/device.[^9]

**13. ARIA roles vs semantic HTML?**
Prefer semantics first (`<nav>`), use ARIA (`role="navigation" aria-label="Main"`) as fallback for custom components.[^11]

**14. How to lazy load images?**
`<img src="placeholder.jpg" loading="lazy" decoding="async">` defers offscreen images, improves LCP performance.[^12]

**15. What is HTML parser blocking?**
`<script>` without `defer/async` blocks rendering. Place at end or use `defer` for DOM-ready scripts.[^9]

**16. Details/Summary element?**

```html
<details>
  <summary>Click to expand</summary>
  <p>Hidden content</p>
</details>
```

Native accordion, accessible by default.[^6]

**17. Picture element?**

```html
<picture>
  <source media="(min-width: 768px)" srcset="large.jpg">
  <img src="small.jpg" alt="Responsive image">
</picture>
```

Art direction + format switching (WebP).[^9]

**18. Template element?**

```
`<template id="row"><tr><td></td></tr></template>` holds inert HTML, clone with JS for dynamic tables.[^6]
```

**19. Dialog element?**

```html
<dialog open>Modal content</dialog>
```

Native modal: `dialog.showModal()` traps focus automatically.[^6]

**20. How to structure accessible form?**

```html
<label for="email">Email</label>
<input id="email" type="email" aria-describedby="help">
<small id="help">Required</small>
```

Explicit associations, descriptions.[^5]

**21. What is Shadow DOM?**
Encapsulated DOM subtree: `<my-component>` styles don't leak. Used in Web Components.[^3]

**22. Contenteditable security risks?**
XSS if user input saved/displayed without sanitization. Use libraries like DOMPurify.[^3]

**23. Microdata/JSON-LD for SEO?**

```
`<script type="application/ld+json">{"@type":"Person","name":"John"}</script>` structured data for rich snippets.[^7]
```

**24. Preconnect/preload?**
`<link rel="preconnect" href="//api.example.com">` warms up critical third-party connections.[^9]

**25. Custom Elements v1?**

```html
<my-button is="button">Click</my-button>
```

Web Components standard for reusable components.[^3]

## CSS Questions (26-50)

**26. CSS Box Model?**
Content + padding + border + margin. `box-sizing: border-box` includes padding/border in width/height.[^8][^3]

**27. Flexbox vs Grid?**
Flexbox: 1D (row/col), content-driven (navbars). Grid: 2D layout (pages), explicit tracks.[^2]

**28. Position: sticky?**
Hybrid: static until threshold (`top: 0`), then fixed. Ex: sticky navbars.[^2]

**29. Z-index requirements?**
Needs `position` other than `static`. Stacks within same stacking context.[^13][^12]

**30. CSS Specificity?**
Inline > ID > Class/Attribute > Element. `!important` overrides. Cascade by order.[^8]

**31. What creates new Stacking Context?**
`position: relative/absolute/fixed z-index`, `opacity <1`, `transform`, `filter`, `isolation: isolate`.[^2]

**32. CSS Custom Properties?**

```css
:root { --primary: #007bff; }
button { color: var(--primary); }
```

Scoped, reactive, cascade-aware variables.[^12]

**33. Container Queries?**

```css
@container (min-width: 400px) { .card { display: grid; } }
```

Style based on parent size, not viewport (2025 standard).[^2]

**34. Cascade Layers?**

```css
@layer base, components, utilities;
@layer components { .btn { } }
```

Explicit cascade control, resets don't override utilities.[^12]

**35. Logical Properties?**
`margin-inline-start` (left in LTR), `padding-block` vs physical `margin-left`. RTL-ready.[^2]

**36. Subgrid?**

```css
.grid { display: grid; grid-template-columns: 1fr 2fr; }
.item { grid-column: subgrid; }
```

Child inherits parent's tracks.[^2]

**37. :has() pseudo-class?**
`section:has(h2)` styles sections containing H2. Parent selectors (2025).[^11]

**38. Viewport units (new)?**
`svh` (small viewport height), `dvh` (dynamic) handle mobile address bar.[^12]

**39. Clamp() for responsive?**
`font-size: clamp(1rem, 2.5vw, 2rem)` fluid typography between min/max.[^12]

**40. Aspect-ratio?**
`aspect-ratio: 16/9` maintains ratios for video/images, replaces padding hack.[^12]

**41. Scroll-driven animations?**
`@scroll-timeline` animates based on scroll position.[^12]

**42. BFC (Block Formatting Context)?**
Created by `overflow: hidden/auto`, `float`, `display: flex/grid`. Contains floats, clears margins.[^2]

**43. CSS-in-JS vs utility classes?**
CSS-in-JS (Emotion): scoped, dynamic. Tailwind: rapid, consistent, larger bundle.[^2]

**44. Critical CSS?**
Inline above-fold styles, async non-critical. Improves Largest Contentful Paint.[^2]

**45. will-change?**
`will-change: transform` hints GPU acceleration, use sparingly (memory cost).[^2]

**46. Reduce reflows?**
Batch DOM reads/writes, use `transform` over `top/left`, `visibility` over `display: none`.[^11]

**47. CSS containment?**
`contain: layout style paint` isolates subtree, boosts performance.[^2]

**48. Preferred vs fallback fonts?**
`font-family: Inter, -apple-system, sans-serif` system stack with custom primary.[^2]

**49. Smooth scrolling?**
`html { scroll-behavior: smooth; }` or `scrollTo({behavior: 'smooth'})`.[^8]

**50. CSS @property?**

```css
@property --slide {
  syntax: "<percentage>";
  initial-value: 0%;
  inherits: false;
}
```

Animatable custom properties with interpolation.[^12]

## Prep Priority

Master Flexbox/Grid, semantic HTML, custom properties, container queries, accessibility for  senior roles. Practice live coding responsive layouts.[^4][^1][^2]
