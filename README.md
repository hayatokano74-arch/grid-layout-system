# Grid Layout System — Claude Code Skill

An industry-standard grid layout system skill for [Claude Code](https://claude.ai/claude-code).
Enforces consistent, accessible, responsive grid rules across all your web projects.

業界標準のグリッドレイアウトシステムスキルです。[Claude Code](https://claude.ai/claude-code) にインストールすると、全てのWebプロジェクトで一貫性のあるレスポンシブなグリッド設計が自動的に適用されます。

---

## Features / 機能

### Core / コア機能
- **8px base grid** — 8pxグリッドを基準としたFluidスペーシング（clamp関数ベース）
- **7 grid types** — 12カラム、非対称、エディトリアル、自動レスポンシブ（RAM）、モジュラー、Subgrid、Bento の7種
- **Fluid typography** — Modular Scaleとclamp()によるFluidタイポグラフィ
- **CJK typography** — 日本語・中国語・韓国語対応（行送り1.7〜2.0、字間、縦書き、font-feature-settings）
- **Dark mode** — WCAG準拠のダークモード配色テンプレート
- **Accessibility** — タッチターゲット48px、テキスト幅65ch、セマンティックHTML等のルール
- **Layout patterns** — ポートフォリオ、スプリットレイアウト、ダッシュボード、LP、フルブリード記事
- **Component generation** — Container, Grid, Section, SplitLayout 等のコンポーネント自動生成
- **Dual output** — 素のCSSとTailwind CSS両方に対応
- **Two modes** — 常時バックグラウンド適用 + `/grid` コマンドによる対話型セットアップ

### v1.1 新機能 / New in v1.1
- **Bento grid** — Apple風の不規則カードレイアウト（span-2, featured等のバリエーション）
- **Responsive images** — `<picture>` / `srcset` / アスペクト比 / 遅延読み込みの完全ガイド
- **Animation & transitions** — View Transitions API、スクロール駆動アニメーション、reduced-motion対応
- **Z-index layer system** — トークンベースの重なり順管理（base → modal → toast の7層）
- **Border & divider system** — ボーダートークン、角丸スケール、セパレータパターン
- **Grid nesting guidelines** — Subgrid vs ネストグリッドの使い分け、最大深度ルール
- **Performance optimization** — CSS containment、content-visibility、will-change の最適化ルール
- **Grid debugging** — ビジュアルオーバーレイ、React デバッグコンポーネント
- **Anti-patterns** — やってはいけないことの包括的リスト（コード例付き）

---

## Installation / インストール

`SKILL.md` を Claude Code のスキルディレクトリにコピーするだけです。

### Global / グローバル（全プロジェクト共通）

```bash
mkdir -p ~/.claude/skills/grid-layout-system
curl -o ~/.claude/skills/grid-layout-system/SKILL.md \
  https://raw.githubusercontent.com/hayatokano74-arch/grid-layout-system/main/SKILL.md
```

### Per-project / プロジェクト単位

```bash
mkdir -p .claude/skills/grid-layout-system
curl -o .claude/skills/grid-layout-system/SKILL.md \
  https://raw.githubusercontent.com/hayatokano74-arch/grid-layout-system/main/SKILL.md
```

---

## Usage / 使い方

### Background Mode / バックグラウンドモード（自動）

インストールするだけで、Claudeがレイアウトコードを書く際に自動的にグリッドルールが適用されます。

Once installed, the skill automatically enforces grid rules whenever Claude writes layout code:

- スペーシングは全て8pxの倍数 / All spacing uses 8px multiples
- タイポグラフィはFluid clamp()値を使用 / Typography uses fluid `clamp()` values
- 日本語テキストには適切な行送り（1.7〜2.0）を適用 / Japanese text gets appropriate line-height
- 色はCSS変数で管理（ダークモード対応） / Colors use CSS custom properties
- テキスト幅は65chを超えない / Text width never exceeds 65ch
- タッチターゲットは最低48x48px / Touch targets are minimum 48x48px
- z-indexはトークンで管理 / Z-index uses token system
- アニメーションはreduced-motion対応 / Animations respect reduced-motion
- 画像は遅延読み込み / Images use lazy loading by default

何もしなくても自動で機能します。 / You don't need to do anything — it just works.

### Interactive Mode / 対話型モード (`/grid`)

Claude Code で `/grid` と入力すると対話型セットアップが始まります。

Type `/grid` in Claude Code to start an interactive setup:

```
You: /grid

Claude: プロジェクトの種類は？ / What type of project?
1. ポートフォリオ / Portfolio
2. SaaS / ダッシュボード / Dashboard
3. ブログ / エディトリアル / Blog
4. ランディングページ / Landing Page
5. ECサイト / E-commerce
6. その他 / Other

You: 1

Claude: フレームワークは？ / Which framework?
1. 素のCSS / Vanilla CSS
2. Tailwind CSS
3. 両方 / Both

You: 2

Claude: コンテンツの主要言語は？ / Primary content language?
1. English
2. 日本語 / Japanese
3. 混在 / Mixed

You: 3

Claude: グリッドシステムを生成中... / Generating grid system...
```

生成されるもの / Claude will generate:
- CSSカスタムプロパティ（スペーシング、タイポグラフィ、色、グリッド設定）
- レイアウトコンポーネント（React/Next.jsプロジェクトの場合）
- 生成されたシステムを使用したサンプルレイアウト

---

## What's Included / 含まれる内容

### Spacing Scale / スペーシングスケール

| Token | Fluid Range | 用途 / Use Case |
|-------|------------|-----------------|
| `--space-xs` | 4px → 8px | アイコン間隔、微調整 / Icon gaps |
| `--space-sm` | 8px → 16px | ラベル間隔、コンパクトリスト / Label spacing |
| `--space-md` | 16px → 32px | 標準パディング / Standard padding |
| `--space-lg` | 24px → 48px | 要素間隔 / Element spacing |
| `--space-xl` | 32px → 64px | セクションパディング / Section padding |
| `--space-2xl` | 48px → 96px | セクション分離 / Section separation |
| `--space-3xl` | 64px → 128px | 大セクション区切り / Major breaks |

### Typography Scale / タイポグラフィスケール

| Token | Fluid Range | 用途 / Use Case |
|-------|------------|-----------------|
| `--font-xs` | 12px → 14px | キャプション、メタデータ / Captions |
| `--font-sm` | 14px → 16px | 補助テキスト / Secondary text |
| `--font-base` | 16px → 18px | 本文 / Body text |
| `--font-lg` | 18px → 24px | 大きめ本文、小見出し / Large body |
| `--font-xl` | 24px → 40px | セクション見出し / Section headings |
| `--font-2xl` | 32px → 56px | ページ見出し / Page headings |
| `--font-3xl` | 40px → 80px | ヒーロー見出し / Hero headings |
| `--font-4xl` | 48px → 112px | ディスプレイ見出し / Display |

### Breakpoints / ブレークポイント

| Name | Width | Columns |
|------|-------|---------|
| Mobile / モバイル | < 640px | 1列 |
| Tablet / タブレット | 640-1023px | 2-3列 |
| Desktop / デスクトップ | 1024-1279px | 3-4列 |
| Wide / ワイド | 1280-1535px | 4-6列 |
| Ultra-wide / 超ワイド | 1536px+ | max-width制限 |

### Grid Types / グリッドの種類

| Type | Best For / 最適な用途 | Key Feature / 特徴 |
|------|---------------------|-------------------|
| **12-column** | 一般的なWebサイト / General websites | 2,3,4,6分割が可能 |
| **Asymmetric** | ポートフォリオ、クリエイティブ / Portfolios | 比率ベース（黄金比、2:1） |
| **Editorial** | ブログ、記事 / Blog, articles | 65ch幅 + フルブリード |
| **Auto-responsive (RAM)** | カードグリッド / Card grids | メディアクエリ不要 |
| **Modular** | ダッシュボード / Dashboards | 行×列のアライメント |
| **Subgrid** | カードレイアウト / Card layouts | コンテナ間の揃え |
| **Bento** | Apple風プロモ / Apple-style promo | 不規則カード、span-2対応 |

### Z-index Layer System / Z-indexレイヤーシステム

| Token | Value | 用途 / Use Case |
|-------|-------|-----------------|
| `--z-base` | 0 | 通常のコンテンツ / Normal content |
| `--z-dropdown` | 100 | ドロップダウン / Dropdowns |
| `--z-sticky` | 200 | 固定ヘッダー / Sticky headers |
| `--z-overlay` | 300 | オーバーレイ背景 / Overlay backdrops |
| `--z-modal` | 400 | モーダル / Modals |
| `--z-popover` | 500 | ポップオーバー / Popovers |
| `--z-toast` | 600 | 通知トースト / Toast notifications |

### Border & Divider System / ボーダー＆ディバイダー

```css
/* ボーダートークン / Border tokens */
--border-thin: 1px solid var(--line-light);
--border-medium: 2px solid var(--line);
--border-thick: 3px solid var(--line);

/* 角丸スケール / Border radius scale */
--radius-sm: 4px;   --radius-md: 8px;
--radius-lg: 12px;  --radius-xl: 16px;
--radius-full: 9999px;
```

### CJK Support / CJKサポート

日本語・中国語・韓国語のタイポグラフィに特化したルールを内蔵しています。

The skill includes specific rules for Japanese/Chinese/Korean typography:

- 行送り / Line-height: 1.7〜2.0（欧文の1.5より広い）
- 字間 / Letter-spacing: 本文 0.04em、見出し 0.08em
- プロポーショナル字詰め / `font-feature-settings: "palt"`
- 縦書き / Vertical writing mode (`writing-mode: vertical-rl`)
- 欧文・和文混植ルール / Mixed content rules (Latin + CJK font stacking)

### Accessibility / アクセシビリティ

WCAG準拠のルールが組み込まれています。

Built-in rules ensure WCAG compliance:

- タッチターゲット / Touch targets: 最小48x48px
- テキスト幅 / Text width: 最大65ch
- 色コントラスト / Color contrast: 本文7:1（AAA）
- セマンティックHTML / Semantic HTML landmarks
- キーボード操作 / Keyboard-accessible focus order
- 論理的なDOM順序 / Logical DOM order matching visual order

### Responsive Images / レスポンシブ画像

画像の最適化ルールが組み込まれています。

Built-in image optimization rules:

- `<picture>` + `<source>` によるフォーマット切り替え（WebP/AVIF）
- `srcset` + `sizes` によるレスポンシブ画像
- アスペクト比の保持（`aspect-ratio` プロパティ）
- 遅延読み込み（`loading="lazy"` + `decoding="async"`）
- ファーストビュー画像は `loading="eager"` + `fetchpriority="high"`

### Animation & Transitions / アニメーション

モダンなアニメーションパターンを内蔵しています。

Modern animation patterns included:

- View Transitions API によるページ遷移
- スクロール駆動アニメーション（`animation-timeline: scroll()`）
- `prefers-reduced-motion` の自動対応
- GPU最適化（`transform` / `opacity` のみ）
- グリッドアニメーション用のイージング関数セット

### Performance / パフォーマンス

CSSパフォーマンス最適化ルールが含まれています。

CSS performance optimization rules:

- `contain: layout style` によるレイアウト分離
- `content-visibility: auto` による画面外レンダリング最適化
- `will-change` の適切な使用ルール（常時指定の禁止）
- グリッドの最大ネスト深度: 3層まで

### Grid Debugging / グリッドデバッグ

開発中のグリッド確認ツールが含まれています。

Development tools for grid verification:

- CSSオーバーレイ（12カラムグリッド可視化）
- Reactデバッグコンポーネント（`<GridDebugOverlay />`）
- ブレークポイントインジケーター（現在のブレークポイントを画面表示）

### Anti-patterns / アンチパターン

やってはいけないことの包括的リストが含まれています。

Comprehensive "what NOT to do" list:

- `grid-template-columns` の固定px値指定の禁止
- `!important` の使用禁止（リセットCSS以外）
- `margin` によるグリッドアイテム配置の禁止
- マジックナンバー（未定義の値）の使用禁止
- 深すぎるネスト（4層以上）の禁止

---

## Example Output / 出力例

### Vanilla CSS

```css
:root {
  --grid-columns: 12;
  --grid-gutter: 1.5rem;
  --grid-margin: clamp(1rem, 4vw, 4rem);
  --grid-max-width: 1200px;

  --space-xs: clamp(0.25rem, 0.18rem + 0.36vw, 0.5rem);
  --space-sm: clamp(0.5rem, 0.36rem + 0.71vw, 1rem);
  --space-md: clamp(1rem, 0.71rem + 1.43vw, 2rem);
  /* ... */

  --font-base: clamp(1rem, 0.93rem + 0.36vw, 1.125rem);
  --font-xl: clamp(1.5rem, 1.14rem + 1.79vw, 2.5rem);
  /* ... */

  --z-base: 0;
  --z-dropdown: 100;
  --z-sticky: 200;
  --z-overlay: 300;
  --z-modal: 400;
  /* ... */
}
```

### Tailwind CSS (v4)

```css
@import "tailwindcss";

@theme {
  --space-xs: clamp(0.25rem, 0.18rem + 0.36vw, 0.5rem);
  --space-sm: clamp(0.5rem, 0.36rem + 0.71vw, 1rem);
  --space-md: clamp(1rem, 0.71rem + 1.43vw, 2rem);
  /* ... */
}
```

### Bento Grid

```css
.bento-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(180px, auto);
  gap: var(--space-md);
}
.bento-grid .featured {
  grid-column: span 2;
  grid-row: span 2;
}
```

### React / Next.js Component

```tsx
<Container size="default">
  <SplitLayout ratio="golden">
    <div>Text content</div>
    <div>Visual content</div>
  </SplitLayout>
</Container>
```

---

## Compatibility / 対応環境

- **Browsers / ブラウザ**: Chrome, Firefox, Safari, Edge（全モダンブラウザ）
- **CSS Features / CSS機能**: CSS Grid, Subgrid, Container Queries, clamp(), Logical Properties, View Transitions API, Scroll-driven Animations, `content-visibility`
- **Frameworks / フレームワーク**: Vanilla CSS, Tailwind CSS v3/v4, React, Next.js, Vue, Astro 等
- **Languages / 言語**: English, Japanese, Chinese, Korean（CJK対応内蔵）

---

## License / ライセンス

MIT

---

## Contributing / コントリビューション

Issue や Pull Request を歓迎します。以下のガイドラインに従ってください。

Issues and pull requests are welcome. Please follow these guidelines:

- SKILL.md のサイズを適切に保つ（Claude Codeがコンテキストに読み込むため）
- 複数のプロジェクトタイプ（ポートフォリオ、SaaS、ブログ）でテストする
- 新しいパターンには素のCSSとTailwindの両方の例を含める
- CJKタイポグラフィルールを維持する
- アンチパターンには必ず正しい代替案を含める / Always include correct alternatives for anti-patterns
- パフォーマンスに影響する変更はベンチマークを添える / Include benchmarks for performance-related changes
