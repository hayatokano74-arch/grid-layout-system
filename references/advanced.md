# Advanced Features — Detailed Reference

## Responsive Image System

### Art Direction with `<picture>`
```html
<picture>
  <source media="(min-width: 1024px)" srcset="hero-wide.webp" />
  <source media="(min-width: 640px)" srcset="hero-medium.webp" />
  <img src="hero-mobile.webp" alt="" loading="lazy" decoding="async" />
</picture>
```

### Aspect Ratio System

| Ratio | Use Case | CSS |
|-------|----------|-----|
| 1:1 | Avatars, thumbnails | `aspect-ratio: 1` |
| 4:3 | Cards, standard photos | `aspect-ratio: 4/3` |
| 3:2 | Landscape photos | `aspect-ratio: 3/2` |
| 16:9 | Videos, hero banners | `aspect-ratio: 16/9` |
| 2:3 | Portrait photos | `aspect-ratio: 2/3` |

```css
.img-container {
  aspect-ratio: 3/2;
  overflow: hidden;
  border-radius: var(--radius-md, 8px);
}
.img-container img {
  width: 100%; height: 100%; object-fit: cover;
}
```

### Responsive Image Sizing
```html
<img
  src="photo.jpg"
  srcset="photo-400.webp 400w, photo-800.webp 800w, photo-1200.webp 1200w"
  sizes="(max-width: 640px) 100vw, (max-width: 1024px) 50vw, 33vw"
  alt="" loading="lazy" decoding="async"
/>
```

### Lazy Loading Strategy
- **Above the fold**: `loading="eager"`, `fetchpriority="high"`
- **Below the fold**: `loading="lazy"` + `decoding="async"`
- **Background images**: `content-visibility: auto` on parent

## Animation & Transitions

### Layout Transition Guidelines
- Duration: 200-300ms UI, 400-600ms page transitions
- Easing: `cubic-bezier(0.4, 0, 0.2, 1)` or `ease-out`
- Only animate `transform` and `opacity` for 60fps
- Always respect `prefers-reduced-motion`

```css
.transition-base {
  transition: transform 200ms ease-out, opacity 200ms ease-out;
}

@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

### View Transitions API
```css
@view-transition { navigation: auto; }
.hero-image { view-transition-name: hero; }
::view-transition-old(hero) { animation: fade-out 300ms ease-out; }
::view-transition-new(hero) { animation: fade-in 300ms ease-in; }
```

### Scroll-driven Animations
```css
.reveal {
  animation: reveal linear both;
  animation-timeline: view();
  animation-range: entry 0% entry 100%;
}
@keyframes reveal {
  from { opacity: 0; transform: translateY(20px); }
  to   { opacity: 1; transform: translateY(0); }
}
```

## Z-index Layer System

```css
:root {
  --z-base:      0;
  --z-raised:    1;
  --z-dropdown:  100;
  --z-sticky:    200;
  --z-overlay:   300;
  --z-modal:     400;
  --z-popover:   500;
  --z-toast:     600;
  --z-max:       9999;   /* Debug overlays only */
}
```

Rules:
- Never use arbitrary z-index — always use tokens
- Use `isolation: isolate` to contain z-index scope
- Sticky elements: use `--z-sticky`, not a random high number

## Border & Divider System

```css
:root {
  --border-thin: 1px;  --border-medium: 2px;  --border-thick: 4px;
  --radius-none: 0;    --radius-sm: 4px;      --radius-md: 8px;
  --radius-lg: 12px;   --radius-xl: 16px;     --radius-full: 9999px;
  --border-default: var(--border-thin) solid var(--color-border-light);
  --border-strong:  var(--border-thin) solid var(--color-border);
}
```

Divider patterns:
```css
.divider { border: 0; border-top: var(--border-default); margin-block: var(--space-lg); }
.divider-vertical { width: var(--border-thin); align-self: stretch; background: var(--color-border-light); }
```

## Grid Nesting Guidelines

| Scenario | Use | Reason |
|----------|-----|--------|
| Card headers/footers must align across cards | **Subgrid** | Children inherit parent lines |
| Dashboard with independent widgets | **Nested Grid** | Own internal structure |
| Form label-input pairs inside page grid | **Subgrid** | Align with page columns |
| Sidebar independent from main grid | **Nested Grid** | Own spacing rules |

Rules:
- Max nesting depth: 3 levels (page → section → component)
- Never nest 2+ grid definitions without subgrid

```css
.page    { display: grid; grid-template-columns: repeat(12, 1fr); gap: var(--grid-gutter); }
.section { grid-column: 1 / -1; display: grid; grid-template-columns: subgrid; }
.card    { display: grid; grid-template-rows: auto 1fr auto; gap: var(--space-sm); }
```

## Performance Optimization

```css
.card         { contain: layout style; }
.lazy-section { content-visibility: auto; contain-intrinsic-size: auto 500px; }
```

Rules:
- Use `content-visibility: auto` for long pages (up to 50% rendering cost reduction)
- Set `contain-intrinsic-size` to prevent CLS
- Use `will-change` only during interaction, remove after
- Prefer `transform: translate()` over `top/left` for animation
- Use `gap` instead of margins between grid items
- Use `<img>` with `width`/`height` attributes (prevents CLS)
- Use WebP/AVIF with `<picture>` fallback

## Grid Debugging

### Visual Overlay (CSS)
```css
.debug-grid {
  background-image: repeating-linear-gradient(90deg,
    rgba(255, 0, 0, 0.05) 0px,
    rgba(255, 0, 0, 0.05) calc((100% - 11 * var(--grid-gutter)) / 12),
    transparent calc((100% - 11 * var(--grid-gutter)) / 12),
    transparent calc((100% - 11 * var(--grid-gutter)) / 12 + var(--grid-gutter))
  );
}
.debug-baseline {
  background-image: linear-gradient(rgba(0, 150, 255, 0.08) 1px, transparent 1px);
  background-size: 100% 8px;
}
```

### Browser DevTools
- Chrome: Elements → click "grid" badge
- Firefox: Layout tab → check grid containers (best inspector)
- Safari: Elements → Layout panel

## Anti-patterns

### Spacing
- NEVER use magic numbers (`margin: 13px`). Use 8px grid scale.
- NEVER mix spacing systems (rem + px + em)

### Grid Layout
- NEVER use `float` for layout
- NEVER nest 3+ grid levels without subgrid
- NEVER use `position: absolute` for what Grid can handle
- NEVER set fixed heights on grid containers — use `min-height`
- NEVER use `!important` on grid properties

### Responsiveness
- NEVER hide content with `display: none` as responsive solution
- NEVER use horizontal scroll for critical mobile content
- NEVER set `max-width` in px on images without `width: 100%`

### Typography
- NEVER set line-height below 1.4 for body (1.7 for CJK)
- NEVER use `px` for font-size in production — use `rem`/`clamp()`
- NEVER exceed 75ch line width
- NEVER use Latin default line-height (1.5) for Japanese text

### Performance
- NEVER apply `will-change` to 2+ elements simultaneously
- NEVER animate `width`/`height`/`margin`/`padding`
- NEVER skip `width`/`height` attributes on `<img>` tags

### Z-index
- NEVER use `z-index: 99999` — use token system
- NEVER set z-index without `position` (no effect on static)
