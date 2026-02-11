# Typography & Color System — Detailed Reference

## Type Scale (Modular Scale)

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

## Line Height

| Language | Body | Heading | Compact |
|----------|------|---------|---------|
| Latin (en) | 1.5 - 1.6 | 1.1 - 1.2 | 1.3 |
| Japanese (ja) | 1.7 - 2.0 | 1.3 - 1.5 | 1.5 |
| Mixed (ja + en) | 1.7 - 1.8 | 1.3 - 1.4 | 1.5 |

## CJK Typography Rules

```css
/* Japanese body text */
.text-ja {
  line-height: 1.8;
  letter-spacing: 0.04em;
  font-feature-settings: "palt";
  word-break: break-all;
  overflow-wrap: anywhere;
  hanging-punctuation: allow-end;
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
  text-orientation: mixed;
  line-height: 1.8;
  letter-spacing: 0.1em;
}

/* Mixed content: CJK + Latin */
.mixed-text {
  line-height: 1.75;
  font-family: "Inter", "Noto Sans JP", sans-serif;
}
```

## Vertical Rhythm (Baseline Grid)

```css
--baseline: 8px;
--rhythm: calc(var(--font-base) * 1.5); /* ~24px for 16px base */
/* All vertical spacing should be multiples of --baseline */
```

## Color System (Light / Dark Mode)

```css
:root {
  --color-bg:          #ffffff;
  --color-bg-subtle:   #f5f5f5;
  --color-bg-muted:    #e8e8e8;
  --color-fg:          #141414;
  --color-fg-muted:    #666666;
  --color-fg-subtle:   #999999;
  --color-border:      #2d2d2d;
  --color-border-light:#d2d2d2;
  --color-accent:      #0066cc;
  --color-accent-hover:#0052a3;
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
