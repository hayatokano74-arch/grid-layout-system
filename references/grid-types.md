# Grid Types — Detailed Reference

## 1. Column Grid (12-column)
The most versatile. Supports 2, 3, 4, 6 column divisions.

```css
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

## 2. Asymmetric Grid
For portfolios, editorial, creative studios. Use ratio-based columns.

```css
/* Golden Ratio (1:1.618) */
.grid-golden { grid-template-columns: 1fr 1.618fr; }

/* Content + Sidebar (2:1) */
.grid-2-1 { grid-template-columns: 2fr 1fr; }

/* Text + Visual (minmax-based) */
.grid-split { grid-template-columns: minmax(280px, 420px) minmax(0, 920px); }
```

## 3. Editorial Grid
Content width limited for readability with full-bleed breakouts.

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

## 4. Auto-responsive Grid (RAM Pattern)
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

## 5. Modular Grid
Row + column structure for dashboards and data-dense UIs.

```css
.grid-modular {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  grid-auto-rows: minmax(200px, auto);
  gap: var(--space-lg);
}
```

## 6. Subgrid
For aligning child elements across sibling containers.

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

## 7. Bento Grid
Irregular card sizes inspired by Apple's design language.

```css
.bento {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(180px, auto);
  gap: var(--space-sm);
}
.bento-wide { grid-column: span 2; }
.bento-tall { grid-row: span 2; }
.bento-hero { grid-column: span 2; grid-row: span 2; }
.bento-full { grid-column: 1 / -1; }

@media (max-width: 768px) {
  .bento { grid-template-columns: repeat(2, 1fr); }
  .bento-hero { grid-column: 1 / -1; }
}
```
```html
<!-- Tailwind -->
<div class="grid grid-cols-4 auto-rows-[minmax(180px,auto)] gap-2 md:grid-cols-2">
  <div class="col-span-2 row-span-2">Hero</div>
  <div class="col-span-2">Wide</div>
  <div class="row-span-2">Tall</div>
</div>
```
