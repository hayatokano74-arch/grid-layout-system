# Grid Layout System Skill

A comprehensive, industry-standard grid layout system for web development.
Provides design rules, spacing systems, typography scales, responsive strategies, and layout component generation.

This skill operates in two modes:
1. **Background mode**: Always enforces grid rules when writing layout code
2. **Interactive mode**: Use `/grid` to generate project-specific grid configurations and components

---

## Core Design Principles

### 8px Base Grid
All spacing, sizing, and layout decisions are based on an 8px grid unit.
Use 4px only for fine adjustments (icon padding, border alignment).

```
Spacing Scale:
4px   (0.25rem)  - Fine adjustment only
8px   (0.5rem)   - XS: icon gaps, tight padding
12px  (0.75rem)  - SM: label-to-field, compact lists
16px  (1rem)     - MD: standard internal padding
24px  (1.5rem)   - LG: element spacing within sections
32px  (2rem)     - XL: card gaps, section padding
48px  (3rem)     - 2XL: section separation
64px  (4rem)     - 3XL: major section breaks
96px  (6rem)     - 4XL: hero padding
128px (8rem)     - 5XL: large section isolation
```

### Fluid Spacing (clamp-based)
Prefer fluid values that scale between breakpoints without media queries:

```css
--space-xs:  clamp(0.25rem, 0.18rem + 0.36vw, 0.5rem);   /*  4px →  8px */
--space-sm:  clamp(0.5rem, 0.36rem + 0.71vw, 1rem);       /*  8px → 16px */
--space-md:  clamp(1rem, 0.71rem + 1.43vw, 2rem);          /* 16px → 32px */
--space-lg:  clamp(1.5rem, 1.07rem + 2.14vw, 3rem);        /* 24px → 48px */
--space-xl:  clamp(2rem, 1.43rem + 2.86vw, 4rem);          /* 32px → 64px */
--space-2xl: clamp(3rem, 2.14rem + 4.29vw, 6rem);          /* 48px → 96px */
--space-3xl: clamp(4rem, 2.86rem + 5.71vw, 8rem);          /* 64px → 128px */
```

---

## Grid Types

Choose the appropriate grid type based on the project:

### 1. Column Grid (12-column)
The most versatile. Supports 2, 3, 4, 6 column divisions.

```css
/* Vanilla CSS */
.grid-12 {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: var(--grid-gutter, 1.5rem);
  max-width: var(--grid-max-width, 1200px);
  margin-inline: auto;
  padding-inline: var(--grid-margin);
}
```

```html
<!-- Tailwind -->
<div class="grid grid-cols-12 gap-6 max-w-[1200px] mx-auto px-[var(--grid-margin)]">
```

### 2. Asymmetric Grid
For portfolios, editorial, creative studios. Use ratio-based columns.

```css
/* Golden Ratio (1:1.618) */
.grid-golden { grid-template-columns: 1fr 1.618fr; }

/* Content + Sidebar (2:1) */
.grid-2-1 { grid-template-columns: 2fr 1fr; }

/* Text + Visual (minmax-based) */
.grid-split { grid-template-columns: minmax(280px, 420px) minmax(0, 920px); }
```

### 3. Editorial Grid
For article/blog pages. Content width limited for readability with full-bleed breakouts.

```css
.editorial {
  display: grid;
  grid-template-columns:
    [full-start] minmax(var(--grid-margin), 1fr)
    [content-start] min(65ch, 100%)
    [content-end] minmax(var(--grid-margin), 1fr)
    [full-end];
}
.editorial > * { grid-column: content; }
.editorial > .wide { grid-column: full; }
```

### 4. Auto-responsive Grid (RAM Pattern)
No media queries needed. Items auto-wrap based on available space.

```css
.grid-auto {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(300px, 100%), 1fr));
  gap: var(--space-lg);
}
```

```html
<!-- Tailwind -->
<div class="grid grid-cols-[repeat(auto-fit,minmax(min(300px,100%),1fr))] gap-6">
```

### 5. Modular Grid
Row + column structure for dashboards and data-dense UIs.

```css
.grid-modular {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  grid-auto-rows: minmax(200px, auto);
  gap: var(--space-lg);
}
```

### 6. Subgrid
For aligning child elements across sibling containers (e.g., card headers/footers).

```css
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: var(--space-lg);
}
.card {
  display: grid;
  grid-template-rows: subgrid;
  grid-row: span 3;
}
```

---

## Breakpoints & Responsive Strategy

### Standard Breakpoints

| Name | Width | Columns | Margin | Gutter |
|------|-------|---------|--------|--------|
| Mobile | < 640px | 1 (max 2) | 16px | 16px |
| Tablet | 640-1023px | 2-3 (8-col grid) | 24px | 24px |
| Desktop | 1024-1279px | 3-4 (12-col grid) | 32px | 24px |
| Wide | 1280-1535px | 4-6 (12-col grid) | 48px | 24px |
| Ultra-wide | 1536px+ | max-width constrained | auto | 24px |

### Fluid Grid Margin

```css
--grid-margin: clamp(1rem, 4vw, 4rem); /* 16px → 64px */
```

### Container Max-widths

| Use case | Max-width |
|----------|-----------|
| Text-optimized (readability) | 65ch (~640px) |
| Standard content | 1200px |
| Wide content | 1440px |
| Full-width | 100% (margin only) |

### Container Queries
Use container queries for component-level responsiveness:

```css
.card-container {
  container-type: inline-size;
}
@container (min-width: 400px) {
  .card-body { grid-template-columns: 1fr 1fr; }
}
```

```html
<!-- Tailwind -->
<div class="@container">
  <div class="@md:grid-cols-2">
```

---

## Typography System

### Type Scale (Modular Scale)

Use **Minor Third (1.2)** for mobile, **Perfect Fourth (1.333)** for desktop:

```css
--font-xs:   clamp(0.75rem, 0.71rem + 0.18vw, 0.875rem);  /* 12px → 14px */
--font-sm:   clamp(0.875rem, 0.83rem + 0.22vw, 1rem);      /* 14px → 16px */
--font-base: clamp(1rem, 0.93rem + 0.36vw, 1.125rem);      /* 16px → 18px */
--font-lg:   clamp(1.125rem, 1rem + 0.63vw, 1.5rem);       /* 18px → 24px */
--font-xl:   clamp(1.5rem, 1.14rem + 1.79vw, 2.5rem);      /* 24px → 40px */
--font-2xl:  clamp(2rem, 1.43rem + 2.86vw, 3.5rem);        /* 32px → 56px */
--font-3xl:  clamp(2.5rem, 1.43rem + 5.36vw, 5rem);        /* 40px → 80px */
--font-4xl:  clamp(3rem, 1.43rem + 7.86vw, 7rem);          /* 48px → 112px */
```

### Line Height

| Language | Body | Heading | Compact |
|----------|------|---------|---------|
| Latin (en) | 1.5 - 1.6 | 1.1 - 1.2 | 1.3 |
| Japanese (ja) | 1.7 - 2.0 | 1.3 - 1.5 | 1.5 |
| Mixed (ja + en) | 1.7 - 1.8 | 1.3 - 1.4 | 1.5 |

### CJK Typography Rules

```css
/* Japanese body text */
.text-ja {
  line-height: 1.8;
  letter-spacing: 0.04em;
  font-feature-settings: "palt";        /* proportional alternates */
  word-break: break-all;                 /* prevent orphan characters */
  overflow-wrap: anywhere;
  hanging-punctuation: allow-end;        /* punctuation outside margins */
}

/* Japanese heading */
.heading-ja {
  line-height: 1.4;
  letter-spacing: 0.08em;
  font-feature-settings: "palt";
}

/* Vertical writing */
.vertical-ja {
  writing-mode: vertical-rl;
  text-orientation: mixed;               /* rotate Latin characters */
  line-height: 1.8;
  letter-spacing: 0.1em;
}

/* Mixed content: CJK + Latin */
.mixed-text {
  line-height: 1.75;
  font-family: "Inter", "Noto Sans JP", sans-serif;  /* Latin first, then CJK */
}
```

### Vertical Rhythm (Baseline Grid)
Align all vertical spacing to a baseline unit:

```css
--baseline: 8px;
--rhythm: calc(var(--font-base) * 1.5); /* ~24px for 16px base */

/* All vertical spacing should be multiples of --baseline */
/* margin-bottom: calc(var(--baseline) * 3) = 24px */
/* margin-bottom: calc(var(--baseline) * 6) = 48px */
```

---

## Color System (Light / Dark Mode)

### CSS Custom Properties Template

```css
:root {
  /* Surfaces */
  --color-bg:          #ffffff;
  --color-bg-subtle:   #f5f5f5;
  --color-bg-muted:    #e8e8e8;

  /* Text */
  --color-fg:          #141414;
  --color-fg-muted:    #666666;
  --color-fg-subtle:   #999999;

  /* Borders */
  --color-border:      #2d2d2d;
  --color-border-light:#d2d2d2;

  /* Interactive */
  --color-accent:      #0066cc;
  --color-accent-hover:#0052a3;

  /* Semantic */
  --color-success:     #16a34a;
  --color-warning:     #ca8a04;
  --color-error:       #dc2626;
}

[data-theme="dark"] {
  --color-bg:          #1a1a1a;
  --color-bg-subtle:   #242424;
  --color-bg-muted:    #333333;

  --color-fg:          #e8e8e8;
  --color-fg-muted:    #888888;
  --color-fg-subtle:   #666666;

  --color-border:      #444444;
  --color-border-light:#3a3a3a;

  --color-accent:      #4d9fff;
  --color-accent-hover:#6db3ff;
}
```

### Dark Mode Contrast Rules
- Body text on background: minimum 7:1 contrast (WCAG AAA)
- Large text (18px+ or 14px+ bold): minimum 4.5:1 contrast (WCAG AA)
- Interactive elements: minimum 3:1 contrast against adjacent colors
- Never use pure black (#000000) on dark mode — use #1a1a1a or darker grays

---

## Accessibility Rules (Grid-related)

### Minimum Sizes
- **Touch targets**: minimum 48x48px (44x44px absolute minimum per WCAG 2.5.8)
- **Click targets**: minimum 24x24px for desktop-only elements
- **Spacing between targets**: minimum 8px gap

### Text Readability
- **Maximum line width**: 65ch for body text (~45-75 characters per line)
- **Minimum font size**: 16px for body text (never below 12px for any text)
- **Line height**: minimum 1.5 for body text

### Semantic HTML with Grid
```html
<!-- Use landmarks, not just divs -->
<header>     <!-- site header -->
<nav>        <!-- navigation -->
<main>       <!-- primary content -->
<aside>      <!-- sidebar -->
<section>    <!-- thematic grouping -->
<article>    <!-- self-contained content -->
<footer>     <!-- site footer -->
```

### Focus & Keyboard
- All interactive elements must be reachable via keyboard (Tab order follows DOM order)
- Grid layout should not break logical reading order
- Use `order` property sparingly — it creates a disconnect between visual and DOM order

---

## Layout Patterns

### Portfolio (image-centric, minimal)
```css
.portfolio-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(350px, 100%), 1fr));
  gap: 4px;  /* minimal gutter for image grids */
}
.portfolio-featured {
  grid-column: span 2;
  grid-row: span 2;
}
```

### Split Layout (text + visual)
```css
.split {
  display: grid;
  grid-template-columns: minmax(280px, 420px) minmax(0, 920px);
  gap: var(--space-xl);
  justify-content: space-between;
  align-items: start;
}
@media (max-width: 900px) {
  .split { grid-template-columns: 1fr; }
}
```

### Dashboard
```css
.dashboard {
  display: grid;
  grid-template-columns: 240px 1fr;
  grid-template-rows: 64px 1fr;
  min-height: 100vh;
}
.dashboard-content {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: var(--space-lg);
  padding: var(--space-lg);
  align-content: start;
}
```

### Landing Page
```css
.hero {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: var(--space-2xl);
  align-items: center;
  min-height: 80vh;
  padding: var(--space-2xl) var(--grid-margin);
}
@media (max-width: 768px) {
  .hero { grid-template-columns: 1fr; min-height: auto; }
}
```

### Full-bleed Article
```css
.article {
  display: grid;
  grid-template-columns:
    [full-start] minmax(var(--grid-margin), 1fr)
    [content-start] min(65ch, 100%)
    [content-end] minmax(var(--grid-margin), 1fr)
    [full-end];
}
.article > *           { grid-column: content; }
.article > .full-bleed { grid-column: full; }
.article > .wide       { grid-column: full; max-width: 1200px; margin-inline: auto; width: 100%; padding-inline: var(--grid-margin); }
```

---

## Component Templates

When asked to generate components, create the following files based on the project's framework:

### Vanilla CSS: `grid-system.css`
Contains all CSS custom properties, grid classes, and responsive rules.

### Tailwind: `tailwind.css` (@theme block)
Contains @theme definitions with all design tokens as CSS variables.

### React/Next.js Components:
- `Container.tsx` — Responsive container with size variants (narrow/default/wide/full)
- `Grid.tsx` — Configurable grid with column count, gap, and responsive props
- `Section.tsx` — Vertical spacing wrapper for page sections
- `SplitLayout.tsx` — Asymmetric two-column layout with ratio options
- `Editorial.tsx` — Article layout with content width + full-bleed support
- `Stack.tsx` — Vertical stack with consistent spacing

---

## /grid Command

When the user invokes `/grid`, follow this interactive flow:

1. **Ask project type**: Portfolio / SaaS / Blog / Landing Page / E-commerce / Other
2. **Ask framework**: Vanilla CSS / Tailwind CSS / Both
3. **Ask language**: English / Japanese / Mixed
4. **Generate**:
   - CSS custom properties (spacing, typography, colors, grid settings)
   - Layout components (if React/Next.js project)
   - Example page layout using the generated system
5. **Output files** to the project's source directory

---

## Rules (Always Active)

When writing any layout code, always follow these rules:

1. **Use 8px grid**: All spacing values must be multiples of 8px (4px for fine adjustments only)
2. **Fluid over fixed**: Prefer `clamp()` over fixed pixel values for spacing and typography
3. **Mobile-first**: Write base styles for mobile, then add complexity with media queries or container queries
4. **Semantic HTML**: Use proper landmark elements, not generic divs
5. **Max text width**: Never let body text exceed 65ch width
6. **Touch targets**: Minimum 48x48px for interactive elements
7. **CJK line-height**: Use 1.7-2.0 for Japanese text, not the Latin default of 1.5
8. **Container queries**: Prefer `@container` over `@media` for component-level responsiveness
9. **Subgrid**: Use `subgrid` for aligning children across sibling containers
10. **Dark mode variables**: Always use CSS custom properties for colors, never hardcoded values
11. **Grid margin**: Use fluid margin `clamp(1rem, 4vw, 4rem)` for page edges
12. **Gutter consistency**: Keep gutters consistent within a layout (default: 24px)
13. **auto-fit vs auto-fill**: Use `auto-fit` for stretching items, `auto-fill` for preserving fixed widths
14. **Aspect ratios**: Use `aspect-ratio` property instead of padding-top hacks
15. **Logical properties**: Prefer `margin-inline`, `padding-block` over directional properties
