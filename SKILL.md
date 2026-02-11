---
name: grid-layout-system
description: Industry-standard grid layout system for web development. Provides 8px-based spacing, fluid typography (clamp), 7 grid types (12-column, asymmetric, editorial, RAM, modular, subgrid, bento), CJK typography rules, dark mode, accessibility, and layout component generation. Use when writing CSS/HTML layout code, creating responsive designs, setting up design tokens, or generating grid-based components. Supports Vanilla CSS and Tailwind CSS. Operates in background mode (auto-enforces rules) and interactive mode (/grid command).
---

# Grid Layout System

## Core Rules (Always Active)

When writing any layout code, enforce these rules:

1. **8px grid**: All spacing = multiples of 8px (4px for fine adjustments only)
2. **Fluid over fixed**: `clamp()` over fixed px for spacing and typography
3. **Mobile-first**: Base styles for mobile, add complexity via media/container queries
4. **Semantic HTML**: Use `<header>`, `<nav>`, `<main>`, `<aside>`, `<section>`, `<article>`, `<footer>`
5. **Max text width**: Body text never exceeds 65ch
6. **Touch targets**: Minimum 48x48px for interactive elements
7. **CJK line-height**: 1.7-2.0 for Japanese, not Latin default 1.5
8. **Container queries**: Prefer `@container` over `@media` for component responsiveness
9. **Subgrid**: Use for aligning children across sibling containers
10. **CSS variables for colors**: Never hardcode color values
11. **Fluid grid margin**: `clamp(1rem, 4vw, 4rem)`
12. **Consistent gutters**: Default 24px within a layout
13. **auto-fit vs auto-fill**: `auto-fit` to stretch, `auto-fill` to preserve widths
14. **Aspect ratios**: Use `aspect-ratio` property, not padding-top hacks
15. **Logical properties**: `margin-inline`/`padding-block` over directional

## Spacing Scale

```
4px  (0.25rem) - Fine adjustment only
8px  (0.5rem)  - XS: icon gaps
16px (1rem)    - MD: standard padding
24px (1.5rem)  - LG: element spacing
32px (2rem)    - XL: section padding
48px (3rem)    - 2XL: section separation
64px (4rem)    - 3XL: major breaks
```

### Fluid Values
```css
--space-xs:  clamp(0.25rem, 0.18rem + 0.36vw, 0.5rem);   /*  4→8px   */
--space-sm:  clamp(0.5rem, 0.36rem + 0.71vw, 1rem);       /*  8→16px  */
--space-md:  clamp(1rem, 0.71rem + 1.43vw, 2rem);          /* 16→32px  */
--space-lg:  clamp(1.5rem, 1.07rem + 2.14vw, 3rem);        /* 24→48px  */
--space-xl:  clamp(2rem, 1.43rem + 2.86vw, 4rem);          /* 32→64px  */
--space-2xl: clamp(3rem, 2.14rem + 4.29vw, 6rem);          /* 48→96px  */
--space-3xl: clamp(4rem, 2.86rem + 5.71vw, 8rem);          /* 64→128px */
```

## Breakpoints

| Name | Width | Columns | Margin |
|------|-------|---------|--------|
| Mobile | <640px | 1-2 | 16px |
| Tablet | 640-1023px | 2-3 | 24px |
| Desktop | 1024-1279px | 3-4 (12-col) | 32px |
| Wide | 1280-1535px | 4-6 (12-col) | 48px |
| Ultra-wide | 1536px+ | max-width limited | auto |

```css
--grid-margin: clamp(1rem, 4vw, 4rem);
```

Container max-widths: text=65ch, standard=1200px, wide=1440px, full=100%.

Use container queries for component-level responsiveness:
```css
.container { container-type: inline-size; }
@container (min-width: 400px) { .inner { grid-template-columns: 1fr 1fr; } }
```

## Grid Types (Quick Reference)

| Type | Best For | Key Pattern |
|------|----------|------------|
| 12-column | General websites | `repeat(12, 1fr)` |
| Asymmetric | Portfolios, creative | `1fr 1.618fr` or `minmax(280px, 420px) minmax(0, 920px)` |
| Editorial | Blog, articles | `min(65ch, 100%)` with full-bleed |
| RAM (auto-responsive) | Card grids | `repeat(auto-fit, minmax(min(300px, 100%), 1fr))` |
| Modular | Dashboards | `repeat(auto-fit, minmax(280px, 1fr))` + `grid-auto-rows` |
| Subgrid | Aligned cards | `grid-template-rows: subgrid; grid-row: span 3` |
| Bento | Apple-style promo | `repeat(4, 1fr)` + span variants |

For detailed code examples of each type, read `references/grid-types.md`.

## Typography (Quick Reference)

Fluid type scale using Minor Third (mobile) → Perfect Fourth (desktop):
```css
--font-base: clamp(1rem, 0.93rem + 0.36vw, 1.125rem);    /* 16→18px */
--font-xl:   clamp(1.5rem, 1.14rem + 1.79vw, 2.5rem);    /* 24→40px */
--font-3xl:  clamp(2.5rem, 1.43rem + 5.36vw, 5rem);       /* 40→80px */
```

CJK essentials:
- Body: `line-height: 1.8; letter-spacing: 0.04em; font-feature-settings: "palt"`
- Heading: `line-height: 1.4; letter-spacing: 0.08em`
- Font stack: `"Inter", "Noto Sans JP", sans-serif` (Latin first, then CJK)

For full type scale, CJK rules, vertical writing, and color system, read `references/typography.md`.

## Accessibility Essentials

- Touch targets: min 48x48px (44px absolute minimum per WCAG 2.5.8)
- Spacing between targets: min 8px
- Max line width: 65ch body text
- Min font size: 16px body (never below 12px)
- Dark mode contrast: body text 7:1 (AAA), large text 4.5:1 (AA)
- Focus/keyboard: Tab order follows DOM order; use `order` sparingly
- Never use pure black (#000) in dark mode — use #1a1a1a

## Key Anti-patterns

- NEVER use magic numbers — always 8px grid scale
- NEVER use `float` for layout — use Grid/Flexbox
- NEVER nest 3+ grid levels without subgrid
- NEVER set fixed heights on grid containers — use `min-height`
- NEVER use Latin line-height (1.5) for Japanese text
- NEVER animate `width`/`height`/`margin` — use `transform`/`opacity`
- NEVER use arbitrary z-index — use token system (`--z-base` through `--z-toast`)
- NEVER skip `width`/`height` on `<img>` tags (causes CLS)

For complete anti-pattern list, read `references/advanced.md`.

## Layout Patterns & Components

For layout patterns (portfolio, split, dashboard, landing page, article) and component templates (Container, Grid, Section, SplitLayout, Editorial, Stack), read `references/components.md`.

## Advanced Features

For responsive images, animation/transitions, View Transitions API, z-index tokens, border/divider system, grid nesting, performance optimization, and debugging tools, read `references/advanced.md`.

## /grid Command

When user invokes `/grid`, follow this interactive flow:

1. Ask project type: Portfolio / SaaS / Blog / Landing Page / E-commerce / Other
2. Ask framework: Vanilla CSS / Tailwind CSS / Both
3. Ask language: English / Japanese / Mixed
4. Generate:
   - CSS custom properties (spacing, typography, colors, grid settings)
   - Layout components (if React/Next.js project detected)
   - Example page layout using the generated system
5. Output files to the project's source directory
