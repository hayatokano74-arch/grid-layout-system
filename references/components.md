# Layout Patterns & Component Templates — Detailed Reference

## Layout Patterns

### Portfolio (image-centric, minimal)
```css
.portfolio-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(350px, 100%), 1fr));
  gap: 4px;
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

## Component Templates

When asked to generate components, create these files based on the project's framework:

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
