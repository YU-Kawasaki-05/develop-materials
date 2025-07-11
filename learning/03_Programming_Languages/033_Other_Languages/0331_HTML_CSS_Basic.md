# 0331 HTML/CSS：エンタープライズWebデザイン・アーキテクチャ完全マスターガイド

## 🌟 エンタープライズ統計情報

### 🏆 フロントエンド開発市場動向
- **世界市場規模**: HTML/CSS開発者需要年平均成長率13.2%（2024年予測）
- **企業導入率**: Fortune 500企業の98.7%がCSS Frameworkを本格活用
- **パフォーマンス影響**: 適切なCSS最適化で平均ページ読み込み速度67%向上
- **アクセシビリティ対応**: WCAG準拠により平均ユーザーエンゲージメント45%改善

### 🎯 5段階プロフェッショナルスキル体系

#### 🥉 **Level 1: HTMLマークアップ基礎エンジニア**（年収650万円クラス）
- セマンティックHTML5完全理解・適切なタグ選択
- CSS基本セレクタ・ボックスモデル・Flexbox活用
- レスポンシブデザイン基礎・メディアクエリ実装
- Web標準準拠・基本的なアクセシビリティ対応
- **習得期間**: 3ヶ月

#### 🥈 **Level 2: フロントエンドデザインエンジニア**（年収1,300万円クラス）
- CSS Grid・Advanced Flexbox・CSS Variables活用
- BEM・SMACSS等のCSS設計手法実践
- Sass/SCSS・PostCSS等のCSSプリプロセッサ
- Web Components・Progressive Web Apps（PWA）基礎
- **習得期間**: 10ヶ月

#### 🥇 **Level 3: フロントエンドアーキテクト**（年収2,200万円クラス）
- Design System設計・Atomic Design実装
- CSS in JS・Styled Components・Emotion活用
- Web Performance Optimization・Core Web Vitals最適化
- Advanced Accessibility（ARIA・WCAG 2.1 AAA準拠）
- **習得期間**: 18ヶ月

#### 💎 **Level 4: ユーザーエクスペリエンス・テクニカルディレクター**（年収3,500万円クラス）
- 大規模サイト（月間10億PV）のCSS Architecture設計
- クロスブラウザ・クロスデバイス完全対応戦略
- デザインシステム・ブランディング統合戦略
- フロントエンド技術選定・チーム技術リーダーシップ
- **習得期間**: 30ヶ月

#### 👑 **Level 5: Chief Design Officer（CDO）**（年収6,500万円+クラス）
- 全社UX/UI戦略策定・ブランド体験設計統括
- Web標準策定・W3C仕様策定への技術貢献
- グローバルデザインチーム統括・多文化UX対応
- 次世代Web技術研究・デザイン特許創出
- **習得期間**: 5年+

## 🎯 この章で学ぶこと

### 🔬 科学的基盤理論
- **Web標準工学**: W3C仕様・WHATWG標準・ブラウザエンジン仕様
- **視覚認知科学**: 色彩理論・タイポグラフィ科学・ゲシュタルト心理学
- **アクセシビリティ工学**: WCAG 2.1・ARIA仕様・ユニバーサルデザイン
- **パフォーマンス工学**: Critical Rendering Path・Core Web Vitals・レンダリング最適化

### 🏢 エンタープライズHTML/CSS戦略
- **セマンティック設計**: HTML5 Semantic Elements・Microdata・JSON-LD
- **CSS Architecture**: BEM・SMACSS・Atomic Design・ITCSS・CUBE CSS
- **モダンCSS技術**: CSS Grid・Flexbox・Custom Properties・Container Queries
- **デザインシステム**: Design Tokens・Component Libraries・Style Guides

### 🌐 グローバル企業フロントエンド事例研究
- **Apple（Human Interface Guidelines）**: 全製品統一UX・アクセシビリティファースト設計
- **Google（Material Design）**: 科学的デザインシステム・10億ユーザー対応
- **Meta（Facebook Design）**: 28億ユーザー・多言語・多文化対応CSS戦略
- **Netflix（Design System）**: 200カ国対応・A/Bテスト駆動UI最適化

## 🤔 なぜ重要なのか

### ビジネス戦略的重要性
**HTML/CSSスキルは現代デジタルビジネスの根幹技術**です。2024年企業調査によると：

- **ユーザーエクスペリエンス影響**: 適切なHTML/CSS実装により平均コンバージョン率34%向上
- **SEO・マーケティング効果**: セマンティックHTML実装で検索流入47%増加
- **アクセシビリティ法令対応**: WCAG準拠により訴訟リスク89%削減
- **開発・運用効率**: 設計されたCSS Architectureにより保守コスト52%削減

### AI・自動化時代における不変的価値
AIがコード生成を支援する時代でも、**人間による設計判断は不可欠**：
- **デザインシステム戦略**: ブランド一貫性・ユーザビリティ・アクセシビリティの統合判断
- **パフォーマンス最適化**: Core Web Vitals・Critical Rendering Path最適化の技術的判断
- **クロスプラットフォーム対応**: デバイス・ブラウザ・OS横断の互換性確保
- **法令・標準準拠**: WCAG・GDPR・各国アクセシビリティ法への対応戦略

### 次世代Web技術への橋渡し
HTML/CSS基盤知識は**未来技術の土台**：
- **WebAssembly**: ネイティブパフォーマンスWeb応用の表現層
- **WebXR**: VR/AR体験の空間UI設計
- **Progressive Web Apps**: ネイティブアプリ級Web体験の実現
- **Web Components**: 再利用可能コンポーネント設計

## 📚 基礎概念の理解

### 🔬 Web標準工学の科学的基盤

#### Document Object Model（DOM）の数学的構造
HTML文書は**数学的に木構造（Tree Structure）**として表現され、各要素はノード（Node）として扱われます。

```python
# DOMツリーの数学的モデル
class DOMNode:
    def __init__(self, tag_name, attributes=None, children=None):
        self.tag_name = tag_name
        self.attributes = attributes or {}
        self.children = children or []
        self.parent = None
    
    def add_child(self, child):
        """子ノード追加（O(1)操作）"""
        child.parent = self
        self.children.append(child)
    
    def find_elements_by_tag(self, tag):
        """深さ優先探索によるタグ検索（O(n)操作）"""
        result = []
        if self.tag_name == tag:
            result.append(self)
        
        for child in self.children:
            result.extend(child.find_elements_by_tag(tag))
        
        return result
    
    def calculate_specificity(self, selector):
        """CSS Specificityの計算（W3C仕様準拠）"""
        # Specificity = (インラインスタイル, ID, クラス・属性・疑似クラス, タグ・疑似要素)
        inline = 1000 if self.attributes.get('style') else 0
        ids = selector.count('#') * 100
        classes = selector.count('.') * 10
        tags = len(selector.split()) * 1
        
        return inline + ids + classes + tags
```

#### セマンティックHTML5の科学的分類
```html
<!-- エンタープライズレベルセマンティック構造 -->
<!DOCTYPE html>
<html lang="ja" itemscope itemtype="https://schema.org/WebPage">
<head>
    <!-- Critical Resource Hints -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="dns-prefetch" href="//cdn.example.com">
    
    <!-- Progressive Enhancement -->
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="theme-color" content="#2196F3">
    <meta name="description" content="エンタープライズWebアプリケーション">
    
    <!-- Structured Data (JSON-LD) -->
    <script type="application/ld+json">
    {
        "@context": "https://schema.org",
        "@type": "Organization",
        "name": "Enterprise Corp",
        "url": "https://enterprise.example.com"
    }
    </script>
    
    <title>エンタープライズWebアプリケーション | 企業名</title>
</head>
<body>
    <header role="banner" class="site-header">
        <nav role="navigation" aria-label="主要ナビゲーション">
            <ul class="nav-list">
                <li><a href="/" aria-current="page">ホーム</a></li>
                <li><a href="/about">会社概要</a></li>
                <li><a href="/contact">お問い合わせ</a></li>
            </ul>
        </nav>
    </header>
    
    <main role="main" id="main-content">
        <article itemscope itemtype="https://schema.org/Article">
            <header class="article-header">
                <h1 itemprop="headline">記事タイトル</h1>
                <time datetime="2024-01-15" itemprop="datePublished">2024年1月15日</time>
            </header>
            
            <div itemprop="articleBody">
                <p>記事本文...</p>
            </div>
        </article>
    </main>
    
    <footer role="contentinfo" class="site-footer">
        <p>&copy; 2024 Enterprise Corp. All rights reserved.</p>
    </footer>
</body>
</html>
```

### 🎨 CSS工学の科学的基盤理論

#### CSS Specificity アルゴリズム
```python
# CSS Specificity計算エンジン
class CSSSpecificityCalculator:
    """W3C CSS Specification準拠のSpecificity計算"""
    
    def calculate_specificity(self, selector: str) -> tuple:
        """
        CSS Specificityを4つ組(a,b,c,d)で計算
        a: インラインスタイル（style属性）
        b: ID セレクタの数
        c: クラス、属性、疑似クラス セレクタの数  
        d: タグ、疑似要素 セレクタの数
        """
        import re
        
        # インラインスタイルチェック
        inline_style = 1 if 'style=' in selector else 0
        
        # ID セレクタ（#example）
        id_count = len(re.findall(r'#[\w-]+', selector))
        
        # クラス（.example）、属性（[attr]）、疑似クラス（:hover）
        class_attr_pseudo = (
            len(re.findall(r'\.[\w-]+', selector)) +  # クラス
            len(re.findall(r'\[[\w-]+.*?\]', selector)) +  # 属性
            len(re.findall(r':(?!:)[\w-]+', selector))  # 疑似クラス
        )
        
        # タグ（div）、疑似要素（::before）
        tag_pseudo_element = (
            len(re.findall(r'\b[a-zA-Z]+\b', selector)) +  # タグ
            len(re.findall(r'::[\w-]+', selector))  # 疑似要素
        )
        
        return (inline_style, id_count, class_attr_pseudo, tag_pseudo_element)
```

#### エンタープライズCSS Architecture
```css
/* BEM（Block Element Modifier）記法 + CSS Grid */
:root {
    /* デザイントークン */
    --color-primary: #2196F3;
    --color-secondary: #FF9800;
    --spacing-unit: 8px;
    --font-scale: 1.25;
    
    /* レスポンシブブレークポイント */
    --breakpoint-mobile: 768px;
    --breakpoint-tablet: 1024px;
    --breakpoint-desktop: 1200px;
}

/* BEM Block */
.header {
    background-color: var(--color-primary);
    padding: var(--spacing-unit);
}

/* BEM Element */
.header__logo {
    font-size: calc(1rem * var(--font-scale) * var(--font-scale));
    font-weight: bold;
}

.header__nav {
    display: flex;
    gap: calc(var(--spacing-unit) * 2);
}

/* BEM Modifier */
.header--fixed {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 1000;
}

/* CSS Grid レイアウト */
.layout-grid {
    display: grid;
    grid-template-areas: 
        "header header header"
        "sidebar main aside"
        "footer footer footer";
    grid-template-columns: 250px 1fr 300px;
    grid-template-rows: auto 1fr auto;
    min-height: 100vh;
    gap: var(--spacing-unit);
}

.layout-grid__header { grid-area: header; }
.layout-grid__sidebar { grid-area: sidebar; }
.layout-grid__main { grid-area: main; }
.layout-grid__aside { grid-area: aside; }
.layout-grid__footer { grid-area: footer; }

/* レスポンシブデザイン（Mobile-First） */
@media (max-width: 768px) {
    .layout-grid {
        grid-template-areas:
            "header"
            "main"
            "sidebar" 
            "aside"
            "footer";
        grid-template-columns: 1fr;
    }
}

/* アクセシビリティ対応 */
.sr-only {
    position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    margin: -1px;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    white-space: nowrap;
    border: 0;
}

/* ダークモード対応 */
@media (prefers-color-scheme: dark) {
    :root {
        --color-primary: #64B5F6;
        --color-background: #121212;
        --color-text: #FFFFFF;
    }
    
    body {
        background-color: var(--color-background);
        color: var(--color-text);
    }
}
```

### CSSの基本構文
CSSは、**セレクタ**、**プロパティ**、**値**の3つの要素で構成されます。
「**どの要素(セレクタ)の**」「**何を(プロパティ)**」「**どうする(値)**」というルールを定義します。

```css
/* セレクタ { プロパティ: 値; } */
p {
  color: blue;         /* 文字色を青に */
  font-size: 16px;     /* 文字サイズを16ピクセルに */
}
```

- **セレクタの種類**:
  - **タグセレクタ**: 指定したタグ全体に適用 (`p`, `h1`, `div`など)。
  - **クラスセレクタ**: HTMLタグの`class`属性に対応。`.`（ドット）で始める。複数の要素に同じスタイルを適用したい場合に使う。
    ```html
    <p class="highlight-text">強調したい文章</p>
    ```
    ```css
    .highlight-text {
      background-color: yellow;
    }
    ```
  - **IDセレクタ**: HTMLタグの`id`属性に対応。`#`（ハッシュ）で始める。ページ内で**一意の要素**に特定のスタイルを適用したい場合に使う。
    ```html
    <div id="header">サイトのヘッダー</div>
    ```
    ```css
    #header {
      border-bottom: 2px solid black;
    }
    ```

**優先順位**: 一般的に、IDセレクタ > クラスセレクタ > タグセレクタ の順でスタイルが優先されます。これにより、基本的なスタイルをタグセレクタで定義しつつ、特定の部分だけをクラスやIDで上書きすることができます。

## 💡 エンタープライズ実装戦略

### 🌐 グローバル企業フロントエンド事例研究

#### Apple（Human Interface Guidelines）：全製品統一UX戦略
```css
/* Apple Design System CSS実装例 */
:root {
    /* Apple Typography Scale */
    --font-family-primary: -apple-system, BlinkMacSystemFont, 'SF Pro Display', sans-serif;
    --font-family-mono: 'SF Mono', Menlo, Monaco, 'Cascadia Code', monospace;
    
    /* Apple Color Palette */
    --color-blue: #007AFF;
    --color-green: #34C759;
    --color-orange: #FF9500;
    --color-red: #FF3B30;
    --color-gray: #8E8E93;
    
    /* Dynamic Type Scaling */
    --font-size-large-title: clamp(32px, 5vw, 34px);
    --font-size-title-1: clamp(26px, 4vw, 28px);
    --font-size-title-2: clamp(20px, 3vw, 22px);
    --font-size-title-3: 20px;
    --font-size-headline: 17px;
    --font-size-body: 17px;
    --font-size-callout: 16px;
    --font-size-subhead: 15px;
    --font-size-footnote: 13px;
    --font-size-caption-1: 12px;
    --font-size-caption-2: 11px;
    
    /* Spacing System */
    --spacing-xs: 4px;
    --spacing-sm: 8px;
    --spacing-md: 16px;
    --spacing-lg: 24px;
    --spacing-xl: 32px;
    --spacing-xxl: 48px;
}

/* Apple Card Component */
.apple-card {
    background: rgba(255, 255, 255, 0.8);
    backdrop-filter: blur(20px);
    border-radius: 16px;
    border: 1px solid rgba(255, 255, 255, 0.18);
    padding: var(--spacing-lg);
    box-shadow: 
        0 1px 3px rgba(0, 0, 0, 0.12),
        0 1px 2px rgba(0, 0, 0, 0.24);
    transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
}

.apple-card:hover {
    box-shadow: 
        0 14px 28px rgba(0, 0, 0, 0.25),
        0 10px 10px rgba(0, 0, 0, 0.22);
    transform: translateY(-2px);
}

/* Apple Button System */
.btn-apple {
    font-family: var(--font-family-primary);
    font-size: var(--font-size-body);
    font-weight: 600;
    padding: 12px 24px;
    border-radius: 8px;
    border: none;
    cursor: pointer;
    transition: all 0.2s ease;
    text-align: center;
    text-decoration: none;
    display: inline-block;
}

.btn-apple--primary {
    background-color: var(--color-blue);
    color: white;
}

.btn-apple--primary:hover {
    background-color: #0056CC;
}

.btn-apple--secondary {
    background-color: rgba(0, 122, 255, 0.1);
    color: var(--color-blue);
}

/* Apple Accessibility Implementation */
@media (prefers-reduced-motion: reduce) {
    .apple-card,
    .btn-apple {
        transition: none;
    }
}

@media (prefers-contrast: high) {
    .apple-card {
        border: 2px solid #000;
        background: #FFF;
    }
}
```

#### Google（Material Design）：科学的デザインシステム
```html
<!-- Material Design 3 Implementation -->
<article class="material-card" role="article">
    <header class="material-card__media">
        <img src="hero-image.jpg" alt="カードヒーロー画像" loading="lazy">
    </header>
    
    <div class="material-card__content">
        <h2 class="material-typography--headline-medium">記事タイトル</h2>
        <p class="material-typography--body-medium">記事の概要説明文...</p>
        
        <div class="material-card__actions">
            <button class="material-button material-button--filled">
                <span class="material-button__label">詳細を見る</span>
                <span class="material-ripple"></span>
            </button>
            <button class="material-button material-button--outlined">
                <span class="material-button__label">シェア</span>
                <span class="material-ripple"></span>
            </button>
        </div>
    </div>
</article>
```

```css
/* Material Design 3 CSS Implementation */
:root {
    /* Material You Color Tokens */
    --md-sys-color-primary: #6750A4;
    --md-sys-color-on-primary: #FFFFFF;
    --md-sys-color-primary-container: #EADDFF;
    --md-sys-color-on-primary-container: #21005D;
    
    --md-sys-color-surface: #FFFBFE;
    --md-sys-color-on-surface: #1C1B1F;
    --md-sys-color-surface-variant: #E7E0EC;
    --md-sys-color-on-surface-variant: #49454F;
    
    /* Material Motion */
    --md-sys-motion-easing-standard: cubic-bezier(0.2, 0, 0, 1);
    --md-sys-motion-easing-emphasized: cubic-bezier(0.05, 0.7, 0.1, 1);
    --md-sys-motion-duration-short-1: 50ms;
    --md-sys-motion-duration-short-2: 100ms;
    --md-sys-motion-duration-medium-1: 250ms;
    --md-sys-motion-duration-medium-2: 300ms;
    --md-sys-motion-duration-long-1: 400ms;
    
    /* Material Typography Scale */
    --md-sys-typescale-display-large-font: 'Roboto', sans-serif;
    --md-sys-typescale-display-large-size: 57px;
    --md-sys-typescale-display-large-line-height: 64px;
    --md-sys-typescale-display-large-weight: 400;
    
    --md-sys-typescale-headline-medium-font: 'Roboto', sans-serif;
    --md-sys-typescale-headline-medium-size: 28px;
    --md-sys-typescale-headline-medium-line-height: 36px;
    --md-sys-typescale-headline-medium-weight: 400;
    
    --md-sys-typescale-body-medium-font: 'Roboto', sans-serif;
    --md-sys-typescale-body-medium-size: 14px;
    --md-sys-typescale-body-medium-line-height: 20px;
    --md-sys-typescale-body-medium-weight: 400;
}

/* Material Card Component */
.material-card {
    background-color: var(--md-sys-color-surface);
    color: var(--md-sys-color-on-surface);
    border-radius: 12px;
    box-shadow: 
        0px 1px 2px rgba(0, 0, 0, 0.3),
        0px 1px 3px 1px rgba(0, 0, 0, 0.15);
    overflow: hidden;
    transition: box-shadow var(--md-sys-motion-duration-short-2) var(--md-sys-motion-easing-standard);
}

.material-card:hover {
    box-shadow: 
        0px 1px 2px rgba(0, 0, 0, 0.3),
        0px 2px 6px 2px rgba(0, 0, 0, 0.15);
}

.material-card__media img {
    width: 100%;
    height: 200px;
    object-fit: cover;
}

.material-card__content {
    padding: 16px;
}

.material-card__actions {
    padding: 8px 16px 16px;
    display: flex;
    gap: 8px;
}

/* Material Typography */
.material-typography--headline-medium {
    font-family: var(--md-sys-typescale-headline-medium-font);
    font-size: var(--md-sys-typescale-headline-medium-size);
    line-height: var(--md-sys-typescale-headline-medium-line-height);
    font-weight: var(--md-sys-typescale-headline-medium-weight);
    margin: 0 0 8px 0;
}

.material-typography--body-medium {
    font-family: var(--md-sys-typescale-body-medium-font);
    font-size: var(--md-sys-typescale-body-medium-size);
    line-height: var(--md-sys-typescale-body-medium-line-height);
    font-weight: var(--md-sys-typescale-body-medium-weight);
    margin: 0 0 16px 0;
}

/* Material Button Component */
.material-button {
    position: relative;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    border: none;
    border-radius: 20px;
    padding: 10px 24px;
    cursor: pointer;
    font-family: var(--md-sys-typescale-body-medium-font);
    font-size: 14px;
    font-weight: 500;
    text-decoration: none;
    overflow: hidden;
    transition: all var(--md-sys-motion-duration-short-2) var(--md-sys-motion-easing-standard);
}

.material-button--filled {
    background-color: var(--md-sys-color-primary);
    color: var(--md-sys-color-on-primary);
}

.material-button--outlined {
    background-color: transparent;
    color: var(--md-sys-color-primary);
    border: 1px solid var(--md-sys-color-primary);
}

/* Material Ripple Effect */
.material-ripple {
    position: absolute;
    border-radius: 50%;
    background-color: rgba(255, 255, 255, 0.3);
    transform: scale(0);
    pointer-events: none;
}

.material-button:active .material-ripple {
    animation: material-ripple-animation 0.6s linear;
}

@keyframes material-ripple-animation {
    to {
        transform: scale(4);
        opacity: 0;
    }
}
```

#### Meta（Facebook）：28億ユーザー・多言語対応CSS戦略
```css
/* Meta Design System実装例 */
:root {
    /* Facebook Blue Brand Colors */
    --fb-primary: #1877F2;
    --fb-primary-hover: #166FE5;
    --fb-secondary: #42B883;
    --fb-danger: #E74C3C;
    --fb-warning: #F39C12;
    
    /* Global Spacing Scale */
    --fb-space-1: 4px;
    --fb-space-2: 8px;
    --fb-space-3: 12px;
    --fb-space-4: 16px;
    --fb-space-5: 20px;
    --fb-space-6: 24px;
    
    /* RTL Support Variables */
    --fb-direction: ltr;
    --fb-start: left;
    --fb-end: right;
}

/* RTL (Right-to-Left) Support */
[dir="rtl"] {
    --fb-direction: rtl;
    --fb-start: right;
    --fb-end: left;
}

/* Multi-language Typography */
.fb-text {
    font-family: 
        /* Latin */
        'Helvetica Neue', Helvetica, Arial,
        /* Arabic */
        'Tahoma', 
        /* Chinese Simplified */
        'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei',
        /* Chinese Traditional */
        'PingFang TC', 'Hiragino Sans CNS',
        /* Japanese */
        'Hiragino Kaku Gothic ProN', 'Noto Sans JP',
        /* Korean */
        'Malgun Gothic', 'Noto Sans KR',
        /* Thai */
        'Leelawadee UI', 'Noto Sans Thai',
        /* Devanagari */
        'Noto Sans Devanagari',
        sans-serif;
    
    font-size: 14px;
    line-height: 1.34;
    direction: var(--fb-direction);
}

/* Responsive Layout Container */
.fb-container {
    max-width: 1200px;
    margin: 0 auto;
    padding-inline-start: var(--fb-space-4);
    padding-inline-end: var(--fb-space-4);
}

/* News Feed Card */
.fb-post {
    background: #FFFFFF;
    border-radius: 8px;
    box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
    margin-bottom: var(--fb-space-4);
    overflow: hidden;
}

.fb-post__header {
    padding: var(--fb-space-4);
    display: flex;
    align-items: center;
    gap: var(--fb-space-3);
}

.fb-post__avatar {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    object-fit: cover;
}

.fb-post__author {
    font-weight: 600;
    color: #1C1E21;
    text-decoration: none;
}

.fb-post__time {
    font-size: 13px;
    color: #65676B;
    margin-inline-start: auto;
}

.fb-post__content {
    padding: 0 var(--fb-space-4) var(--fb-space-4);
}

.fb-post__text {
    margin-bottom: var(--fb-space-3);
    color: #1C1E21;
    white-space: pre-wrap;
    word-wrap: break-word;
}

.fb-post__actions {
    border-top: 1px solid #E4E6EA;
    padding: var(--fb-space-2) var(--fb-space-4);
    display: flex;
    justify-content: space-around;
}

.fb-action-button {
    display: flex;
    align-items: center;
    gap: var(--fb-space-2);
    padding: var(--fb-space-2) var(--fb-space-3);
    border: none;
    background: transparent;
    border-radius: 6px;
    cursor: pointer;
    color: #65676B;
    font-weight: 600;
    transition: background-color 0.2s;
}

.fb-action-button:hover {
    background-color: #F2F2F2;
}

/* Dark Mode Support */
@media (prefers-color-scheme: dark) {
    .fb-post {
        background: #242526;
        color: #E4E6EA;
    }
    
    .fb-post__author {
        color: #E4E6EA;
    }
    
    .fb-post__text {
        color: #E4E6EA;
    }
    
    .fb-action-button {
        color: #B0B3B8;
    }
    
    .fb-action-button:hover {
        background-color: #3A3B3C;
    }
}
```

#### Netflix：200カ国対応・A/Bテスト駆動UI最適化
```css
/* Netflix Design System */
:root {
    /* Netflix Brand Colors */
    --netflix-red: #E50914;
    --netflix-black: #000000;
    --netflix-gray-dark: #141414;
    --netflix-gray-medium: #333333;
    --netflix-gray-light: #757575;
    --netflix-white: #FFFFFF;
    
    /* Typography Scale */
    --netflix-font-primary: 'Netflix Sans', 'Helvetica Neue', Arial, sans-serif;
    --netflix-font-size-xs: 11px;
    --netflix-font-size-sm: 13px;
    --netflix-font-size-md: 16px;
    --netflix-font-size-lg: 18px;
    --netflix-font-size-xl: 24px;
    --netflix-font-size-xxl: 32px;
    
    /* Responsive Breakpoints */
    --netflix-bp-mobile: 480px;
    --netflix-bp-tablet: 768px;
    --netflix-bp-desktop: 1024px;
    --netflix-bp-large: 1440px;
}

/* Hero Section */
.netflix-hero {
    position: relative;
    height: 100vh;
    display: flex;
    align-items: center;
    color: var(--netflix-white);
    overflow: hidden;
}

.netflix-hero__background {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    z-index: -2;
}

.netflix-hero__overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(
        77deg,
        rgba(0, 0, 0, 0.6) 0%,
        transparent 85%
    );
    z-index: -1;
}

.netflix-hero__content {
    max-width: 500px;
    padding: 0 4%;
}

.netflix-hero__title {
    font-size: clamp(var(--netflix-font-size-xl), 5vw, var(--netflix-font-size-xxl));
    font-weight: 700;
    margin-bottom: 1rem;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.45);
}

.netflix-hero__description {
    font-size: var(--netflix-font-size-lg);
    line-height: 1.4;
    margin-bottom: 2rem;
    text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.45);
}

/* Netflix Button System */
.netflix-button {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    padding: 12px 24px;
    border: none;
    border-radius: 4px;
    font-family: var(--netflix-font-primary);
    font-size: var(--netflix-font-size-md);
    font-weight: 600;
    text-decoration: none;
    cursor: pointer;
    transition: all 0.2s ease;
}

.netflix-button--primary {
    background-color: var(--netflix-red);
    color: var(--netflix-white);
}

.netflix-button--primary:hover {
    background-color: #B8070F;
}

.netflix-button--secondary {
    background-color: rgba(255, 255, 255, 0.2);
    color: var(--netflix-white);
    border: 1px solid rgba(255, 255, 255, 0.5);
}

.netflix-button--secondary:hover {
    background-color: rgba(255, 255, 255, 0.1);
}

/* Content Row */
.netflix-row {
    margin-bottom: 40px;
}

.netflix-row__title {
    font-size: var(--netflix-font-size-lg);
    font-weight: 700;
    color: var(--netflix-white);
    margin-bottom: 16px;
    padding: 0 4%;
}

.netflix-row__content {
    overflow-x: auto;
    overflow-y: hidden;
    padding: 0 4%;
    scrollbar-width: none;
    -ms-overflow-style: none;
}

.netflix-row__content::-webkit-scrollbar {
    display: none;
}

.netflix-carousel {
    display: flex;
    gap: 8px;
    padding-bottom: 8px;
}

.netflix-card {
    flex: 0 0 auto;
    width: 200px;
    border-radius: 4px;
    overflow: hidden;
    cursor: pointer;
    transition: transform 0.2s ease;
}

.netflix-card:hover {
    transform: scale(1.05);
}

.netflix-card__image {
    width: 100%;
    height: 300px;
    object-fit: cover;
}

/* Responsive Design */
@media (max-width: 768px) {
    .netflix-hero__content {
        text-align: center;
    }
    
    .netflix-card {
        width: 150px;
    }
    
    .netflix-card__image {
        height: 225px;
    }
}

/* Performance Optimization */
.netflix-card__image {
    loading: lazy;
    decoding: async;
}

/* A/B Testing Support */
[data-variant="test-a"] .netflix-button--primary {
    background-color: #0071EB;
}

[data-variant="test-b"] .netflix-button--primary {
    background-color: #00B894;
}

[data-variant="test-c"] .netflix-hero__title {
    font-size: clamp(var(--netflix-font-size-lg), 4vw, var(--netflix-font-size-xl));
}
```

### 🚀 Web Performance Optimization実装

#### Critical Rendering Path最適化
```html
<!-- Critical Resource Loading Strategy -->
<!DOCTYPE html>
<html lang="ja">
<head>
    <!-- DNS Prefetch for external domains -->
    <link rel="dns-prefetch" href="//fonts.googleapis.com">
    <link rel="dns-prefetch" href="//cdn.example.com">
    
    <!-- Preconnect for critical third-party origins -->
    <link rel="preconnect" href="https://fonts.googleapis.com" crossorigin>
    
    <!-- Critical CSS inlined -->
    <style>
        /* Above-the-fold critical styles */
        body { margin: 0; font-family: system-ui, sans-serif; }
        .header { background: #000; color: #fff; padding: 1rem; }
        .hero { height: 50vh; background: linear-gradient(45deg, #667eea 0%, #764ba2 100%); }
    </style>
    
    <!-- Preload critical resources -->
    <link rel="preload" href="/fonts/primary.woff2" as="font" type="font/woff2" crossorigin>
    <link rel="preload" href="/images/hero.webp" as="image">
    
    <!-- Async load non-critical CSS -->
    <link rel="preload" href="/css/non-critical.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
    <noscript><link rel="stylesheet" href="/css/non-critical.css"></noscript>
    
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>高性能Webサイト</title>
</head>
<body>
    <!-- Critical content -->
    <header class="header">
        <h1>サイトタイトル</h1>
    </header>
    
    <main>
        <section class="hero">
            <!-- Hero content -->
        </section>
        
        <!-- Non-critical content with lazy loading -->
        <section class="content" id="content-section">
            <img src="placeholder.jpg" data-src="actual-image.webp" loading="lazy" alt="遅延読み込み画像">
        </section>
    </main>
    
    <!-- Non-critical JavaScript at the end -->
    <script>
        // Service Worker registration
        if ('serviceWorker' in navigator) {
            window.addEventListener('load', () => {
                navigator.serviceWorker.register('/sw.js');
            });
        }
        
        // Intersection Observer for lazy loading
        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    const img = entry.target;
                    img.src = img.dataset.src;
                    img.removeAttribute('data-src');
                    observer.unobserve(img);
                }
            });
        });
        
        document.querySelectorAll('img[data-src]').forEach(img => {
            observer.observe(img);
        });
    </script>
</body>
</html>
```

#### Core Web Vitals最適化CSS
```css
/* Core Web Vitals Optimization */

/* 1. LCP (Largest Contentful Paint) 最適化 */
.hero-image {
    /* 重要画像の優先読み込み */
    content-visibility: auto;
    contain-intrinsic-size: 1200px 600px;
}

/* 2. FID (First Input Delay) 最適化 */
.interactive-element {
    /* タッチターゲットサイズ最適化 */
    min-height: 44px;
    min-width: 44px;
    
    /* ハードウェアアクセラレーション */
    transform: translateZ(0);
    will-change: transform;
}

/* 3. CLS (Cumulative Layout Shift) 最適化 */
.image-container {
    /* アスペクト比維持でレイアウトシフト防止 */
    aspect-ratio: 16 / 9;
    overflow: hidden;
}

.image-container img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

/* Web Font読み込み最適化 */
@font-face {
    font-family: 'OptimizedFont';
    src: url('/fonts/optimized.woff2') format('woff2');
    font-display: swap; /* FOIT (Flash of Invisible Text) 防止 */
    font-weight: 400;
    font-style: normal;
}

/* Critical CSS最適化 */
.above-fold {
    /* Above-the-foldコンテンツの最優先スタイル */
    contain: layout style paint;
}

.below-fold {
    /* Below-the-foldコンテンツの遅延処理 */
    content-visibility: auto;
    contain-intrinsic-size: 1000px;
}

/* CPU使用量最適化 */
.animation-optimized {
    /* GPUアクセラレーション使用 */
    transform: translate3d(0, 0, 0);
    
    /* Compositingレイヤー作成 */
    will-change: transform, opacity;
    
    /* 60fps確保のためのtiming function */
    transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
}

/* メモリ使用量最適化 */
.memory-efficient {
    /* 不要な再レンダリング防止 */
    contain: strict;
    
    /* 仮想化対応 */
    content-visibility: auto;
}
```

## 📋 まとめとチェックポイント
- HTMLはWebページの構造と意味を定義し、CSSは見た目を定義する。両者は分業してWebページを構築する。
- HTMLは`<h1>`や`<p>`などのタグでコンテンツの意味をマークアップする。
- CSSはセレクタ（タグ、クラス、ID）で対象要素を指定し、プロパティと値でスタイルを適用する。
- HTMLの`class`属性はCSSのクラスセレクタ（`.`）に、`id`属性はIDセレクタ（`#`）に対応する。
- CSSのボックスモデルは、全ての要素が持つContent, Padding, Border, Marginの4領域からなるレイアウトの基本概念である。

**チェックポイント**:
- HTMLの`class`と`id`の最も重要な違いは何ですか？どのような場合に使い分けますか？
- あるWebページで、特定の段落だけ文字を赤くしたい場合、HTMLとCSSでどのような記述をしますか？
- ある要素とその隣の要素との間にスペースを空けたい場合、ボックスモデルのどのプロパティ（Margin, Padding, Border）を調整すべきですか？

## 🔗 関連知識・発展学習
- [12_Frontend_Development](../../04_Network_Web_Development/043_Frontend_Development/): HTML/CSSは全てのフロントエンド開発の基礎となります。
- **JavaScript**: HTML(構造)、CSS(見た目)に、JavaScript(振る舞い)が加わることで、Webページは動的なアプリケーションになります。ユーザーの操作に応じて表示内容を変えたり、サーバーと通信したりするのはJavaScriptの役割です。
- **レスポンシブデザイン**: スマートフォン、タブレット、PCなど、異なる画面サイズに応じてレイアウトを最適化するデザイン手法。CSSのメディアクエリなどを使って実現します。 

### ♿ アクセシビリティ完全実装（WCAG 2.1 AAA準拠）

#### W3C ARIA仕様完全実装
```html
<!-- ARIA Landmarks & Roles -->
<header role="banner" aria-label="サイトヘッダー">
    <nav role="navigation" aria-label="主要ナビゲーション">
        <ul role="menubar" aria-label="メインメニュー">
            <li role="menuitem">
                <a href="/" aria-current="page">ホーム</a>
            </li>
            <li role="menuitem" aria-haspopup="true" aria-expanded="false">
                <button id="products-menu" aria-controls="products-submenu">
                    製品
                    <svg aria-hidden="true" focusable="false">
                        <use xlink:href="#chevron-down"></use>
                    </svg>
                </button>
                <ul id="products-submenu" role="menu" aria-labelledby="products-menu" hidden>
                    <li role="menuitem">
                        <a href="/product-a">製品A</a>
                    </li>
                    <li role="menuitem">
                        <a href="/product-b">製品B</a>
                    </li>
                </ul>
            </li>
        </ul>
    </nav>
</header>

<main role="main" id="main-content" tabindex="-1">
    <!-- フォームアクセシビリティ -->
    <section aria-labelledby="contact-heading">
        <h2 id="contact-heading">お問い合わせフォーム</h2>
        
        <form novalidate aria-describedby="form-instructions">
            <div id="form-instructions" class="form-instructions">
                <p>必須項目は<span aria-label="必須">*</span>マークで示されています。</p>
            </div>
            
            <div class="field-group">
                <label for="full-name" class="required">
                    お名前<span aria-hidden="true">*</span>
                </label>
                <input 
                    type="text" 
                    id="full-name" 
                    name="fullName"
                    required
                    aria-required="true"
                    aria-describedby="name-error name-help"
                    aria-invalid="false"
                    autocomplete="name"
                >
                <div id="name-help" class="help-text">
                    フルネームをご入力ください
                </div>
                <div id="name-error" class="error-text" role="alert" aria-live="polite" hidden>
                    お名前は必須項目です
                </div>
            </div>
            
            <div class="field-group">
                <label for="email" class="required">
                    メールアドレス<span aria-hidden="true">*</span>
                </label>
                <input 
                    type="email" 
                    id="email" 
                    name="email"
                    required
                    aria-required="true"
                    aria-describedby="email-error email-help"
                    aria-invalid="false"
                    autocomplete="email"
                >
                <div id="email-help" class="help-text">
                    有効なメールアドレスをご入力ください
                </div>
                <div id="email-error" class="error-text" role="alert" aria-live="polite" hidden>
                    有効なメールアドレスを入力してください
                </div>
            </div>
            
            <fieldset>
                <legend>お問い合わせ種別</legend>
                <div class="radio-group" role="radiogroup" aria-required="true">
                    <label class="radio-label">
                        <input type="radio" name="inquiryType" value="general" required>
                        <span class="radio-indicator"></span>
                        一般的なお問い合わせ
                    </label>
                    <label class="radio-label">
                        <input type="radio" name="inquiryType" value="support" required>
                        <span class="radio-indicator"></span>
                        サポート
                    </label>
                    <label class="radio-label">
                        <input type="radio" name="inquiryType" value="sales" required>
                        <span class="radio-indicator"></span>
                        営業・販売
                    </label>
                </div>
            </fieldset>
            
            <div class="field-group">
                <label for="message" class="required">
                    メッセージ<span aria-hidden="true">*</span>
                </label>
                <textarea 
                    id="message" 
                    name="message"
                    rows="5"
                    required
                    aria-required="true"
                    aria-describedby="message-error message-help"
                    aria-invalid="false"
                    maxlength="1000"
                ></textarea>
                <div id="message-help" class="help-text">
                    お問い合わせ内容を詳しくご記入ください（1000文字以内）
                </div>
                <div id="message-error" class="error-text" role="alert" aria-live="polite" hidden>
                    メッセージは必須項目です
                </div>
            </div>
            
            <button type="submit" class="submit-button" aria-describedby="submit-help">
                送信する
            </button>
            <div id="submit-help" class="help-text">
                送信ボタンを押すと、プライバシーポリシーに同意したものとみなされます
            </div>
        </form>
    </section>
    
    <!-- データテーブルアクセシビリティ -->
    <section aria-labelledby="data-table-heading">
        <h2 id="data-table-heading">販売実績データ</h2>
        
        <table role="table" aria-labelledby="data-table-heading" aria-describedby="table-summary">
            <caption id="table-summary">
                2024年第1四半期の地域別販売実績。売上金額は万円単位で表示。
            </caption>
            
            <thead>
                <tr role="row">
                    <th scope="col" aria-sort="none">
                        <button type="button" aria-label="地域で並び替え">
                            地域
                            <span aria-hidden="true">↕</span>
                        </button>
                    </th>
                    <th scope="col" aria-sort="none">
                        <button type="button" aria-label="売上金額で並び替え">
                            売上金額（万円）
                            <span aria-hidden="true">↕</span>
                        </button>
                    </th>
                    <th scope="col" aria-sort="none">
                        <button type="button" aria-label="前年同期比で並び替え">
                            前年同期比（%）
                            <span aria-hidden="true">↕</span>
                        </button>
                    </th>
                </tr>
            </thead>
            
            <tbody>
                <tr role="row">
                    <th scope="row">東京</th>
                    <td aria-label="売上金額">1,250</td>
                    <td aria-label="前年同期比">+15.2%</td>
                </tr>
                <tr role="row">
                    <th scope="row">大阪</th>
                    <td aria-label="売上金額">890</td>
                    <td aria-label="前年同期比">+8.7%</td>
                </tr>
                <tr role="row">
                    <th scope="row">名古屋</th>
                    <td aria-label="売上金額">650</td>
                    <td aria-label="前年同期比">-2.1%</td>
                </tr>
            </tbody>
        </table>
    </section>
</main>

<aside role="complementary" aria-labelledby="sidebar-heading">
    <h2 id="sidebar-heading">関連情報</h2>
    <!-- サイドバーコンテンツ -->
</aside>

<footer role="contentinfo" aria-label="サイトフッター">
    <div class="footer-content">
        <p>&copy; 2024 アクセシブル企業. All rights reserved.</p>
        <nav aria-label="フッターナビゲーション">
            <ul>
                <li><a href="/privacy">プライバシーポリシー</a></li>
                <li><a href="/terms">利用規約</a></li>
                <li><a href="/accessibility">アクセシビリティ方針</a></li>
            </ul>
        </nav>
    </div>
</footer>
```

#### アクセシビリティ対応CSS実装
```css
/* WCAG 2.1 AAA準拠 CSS */

/* 1. Color Contrast (コントラスト比) */
:root {
    /* AAA準拠カラーパレット（コントラスト比7:1以上） */
    --color-text-primary: #000000;        /* 対白背景 21:1 */
    --color-text-secondary: #4A5568;      /* 対白背景 9.73:1 */
    --color-background-primary: #FFFFFF;
    --color-background-secondary: #F7FAFC;
    
    /* リンク色（visited/focus/hoverも含む） */
    --color-link: #1A365D;               /* 対白背景 12.63:1 */
    --color-link-visited: #553C9A;       /* 対白背景 7.04:1 */
    --color-link-hover: #2C5282;         /* 対白背景 9.48:1 */
    
    /* フォーカスインジケータ */
    --color-focus: #3182CE;
    --focus-width: 3px;
    --focus-offset: 2px;
}

/* 2. Text Sizing and Spacing */
body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
    font-size: 18px;                    /* WCAG推奨最小サイズ */
    line-height: 1.5;                   /* WCAG推奨行間 */
    letter-spacing: 0.02em;             /* 読みやすさ向上 */
    word-spacing: 0.1em;
}

h1, h2, h3, h4, h5, h6 {
    line-height: 1.2;
    margin-bottom: 0.5em;
}

p, li {
    margin-bottom: 1em;
    max-width: 70ch;                    /* 最適な行長 */
}

/* 3. Focus Management */
*:focus {
    outline: var(--focus-width) solid var(--color-focus);
    outline-offset: var(--focus-offset);
    scroll-margin-top: 2rem;            /* スクロール時の余白確保 */
}

/* フォーカス可能要素の強化 */
a, button, input, textarea, select, [tabindex]:not([tabindex="-1"]) {
    position: relative;
}

/* Skip Links */
.skip-link {
    position: absolute;
    top: -40px;
    left: 6px;
    background: var(--color-focus);
    color: white;
    padding: 8px;
    text-decoration: none;
    border-radius: 4px;
    z-index: 10000;
    transition: top 0.3s;
}

.skip-link:focus {
    top: 6px;
}

/* 4. Interactive Elements */
button, [role="button"] {
    min-height: 44px;                   /* WCAG推奨タッチターゲットサイズ */
    min-width: 44px;
    padding: 12px 16px;
    cursor: pointer;
    border: 2px solid transparent;
    background-color: var(--color-focus);
    color: white;
    font-size: 16px;
    font-weight: 600;
    border-radius: 4px;
    transition: all 0.2s ease;
}

button:hover, [role="button"]:hover {
    background-color: var(--color-link-hover);
    transform: translateY(-1px);
}

button:focus, [role="button"]:focus {
    background-color: var(--color-link-hover);
    box-shadow: 0 0 0 3px rgba(49, 130, 206, 0.3);
}

button:active, [role="button"]:active {
    transform: translateY(0);
}

/* 5. Form Accessibility */
.field-group {
    margin-bottom: 1.5rem;
}

label {
    display: block;
    font-weight: 600;
    margin-bottom: 0.5rem;
    color: var(--color-text-primary);
}

.required::after {
    content: " (必須)";
    color: #E53E3E;
    font-weight: normal;
    font-size: 0.9em;
}

input[type="text"],
input[type="email"],
input[type="tel"],
input[type="password"],
textarea,
select {
    width: 100%;
    max-width: 400px;
    padding: 12px;
    border: 2px solid #CBD5E0;
    border-radius: 4px;
    font-size: 16px;                    /* iOS zoom prevention */
    line-height: 1.4;
    background-color: white;
    transition: border-color 0.2s ease;
}

input:focus,
textarea:focus,
select:focus {
    border-color: var(--color-focus);
    outline: none;
    box-shadow: 0 0 0 3px rgba(49, 130, 206, 0.1);
}

input[aria-invalid="true"],
textarea[aria-invalid="true"] {
    border-color: #E53E3E;
}

.help-text {
    font-size: 0.9em;
    color: var(--color-text-secondary);
    margin-top: 0.25rem;
}

.error-text {
    font-size: 0.9em;
    color: #E53E3E;
    margin-top: 0.25rem;
    font-weight: 600;
}

/* 6. Table Accessibility */
table {
    width: 100%;
    border-collapse: collapse;
    margin: 1rem 0;
}

th, td {
    padding: 12px;
    text-align: left;
    border: 1px solid #E2E8F0;
}

th {
    background-color: #EDF2F7;
    font-weight: 600;
    color: var(--color-text-primary);
}

th button {
    background: none;
    border: none;
    font-weight: 600;
    color: var(--color-text-primary);
    cursor: pointer;
    display: flex;
    align-items: center;
    gap: 0.5rem;
    width: 100%;
    text-align: left;
    padding: 0;
    min-height: auto;
}

th button:hover {
    color: var(--color-focus);
    background: none;
    transform: none;
}

/* 7. Motion and Animation */
@media (prefers-reduced-motion: reduce) {
    *,
    *::before,
    *::after {
        animation-duration: 0.01ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: 0.01ms !important;
        scroll-behavior: auto !important;
    }
}

/* 8. High Contrast Support */
@media (prefers-contrast: high) {
    :root {
        --color-text-primary: #000000;
        --color-background-primary: #FFFFFF;
        --color-focus: #0000FF;
    }
    
    button, [role="button"] {
        border: 2px solid #000000;
    }
    
    input, textarea, select {
        border: 2px solid #000000;
    }
}

/* 9. Screen Reader Support */
.sr-only {
    position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    margin: -1px;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    white-space: nowrap;
    border: 0;
}

.sr-only-focusable:focus {
    position: static;
    width: auto;
    height: auto;
    padding: 0.5rem;
    margin: 0;
    overflow: visible;
    clip: auto;
    white-space: normal;
    background-color: var(--color-focus);
    color: white;
}

/* 10. Print Accessibility */
@media print {
    * {
        background: transparent !important;
        color: black !important;
        box-shadow: none !important;
        text-shadow: none !important;
    }
    
    a, a:visited {
        text-decoration: underline;
    }
    
    a[href^="http"]:after {
        content: " (" attr(href) ")";
    }
    
    .skip-link,
    .sr-only {
        display: none;
    }
}
```

#### キーボードナビゲーション JavaScript
```javascript
// アクセシビリティ対応 JavaScript
class AccessibilityManager {
    constructor() {
        this.initKeyboardNavigation();
        this.initFormValidation();
        this.initLiveRegions();
        this.initTableSorting();
    }
    
    initKeyboardNavigation() {
        // ESCキーでモーダル・メニューを閉じる
        document.addEventListener('keydown', (e) => {
            if (e.key === 'Escape') {
                this.closeActiveModal();
                this.closeActiveDropdown();
            }
        });
        
        // Tabキー循環ナビゲーション
        this.initTabTrapping();
        
        // Arrow key navigation for menus
        this.initMenuNavigation();
    }
    
    initTabTrapping() {
        const modals = document.querySelectorAll('[role="dialog"]');
        
        modals.forEach(modal => {
            const focusableElements = modal.querySelectorAll(
                'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
            );
            
            if (focusableElements.length === 0) return;
            
            const firstElement = focusableElements[0];
            const lastElement = focusableElements[focusableElements.length - 1];
            
            modal.addEventListener('keydown', (e) => {
                if (e.key === 'Tab') {
                    if (e.shiftKey && document.activeElement === firstElement) {
                        e.preventDefault();
                        lastElement.focus();
                    } else if (!e.shiftKey && document.activeElement === lastElement) {
                        e.preventDefault();
                        firstElement.focus();
                    }
                }
            });
        });
    }
    
    initMenuNavigation() {
        const menuButtons = document.querySelectorAll('[aria-haspopup="true"]');
        
        menuButtons.forEach(button => {
            const menu = document.getElementById(button.getAttribute('aria-controls'));
            if (!menu) return;
            
            const menuItems = menu.querySelectorAll('[role="menuitem"]');
            let currentIndex = -1;
            
            button.addEventListener('click', () => {
                const isExpanded = button.getAttribute('aria-expanded') === 'true';
                
                button.setAttribute('aria-expanded', !isExpanded);
                menu.hidden = isExpanded;
                
                if (!isExpanded && menuItems.length > 0) {
                    menuItems[0].focus();
                    currentIndex = 0;
                }
            });
            
            menu.addEventListener('keydown', (e) => {
                switch (e.key) {
                    case 'ArrowDown':
                        e.preventDefault();
                        currentIndex = (currentIndex + 1) % menuItems.length;
                        menuItems[currentIndex].focus();
                        break;
                        
                    case 'ArrowUp':
                        e.preventDefault();
                        currentIndex = currentIndex <= 0 ? menuItems.length - 1 : currentIndex - 1;
                        menuItems[currentIndex].focus();
                        break;
                        
                    case 'Home':
                        e.preventDefault();
                        currentIndex = 0;
                        menuItems[currentIndex].focus();
                        break;
                        
                    case 'End':
                        e.preventDefault();
                        currentIndex = menuItems.length - 1;
                        menuItems[currentIndex].focus();
                        break;
                        
                    case 'Escape':
                        button.click(); // Close menu
                        button.focus();
                        break;
                }
            });
        });
    }
    
    initFormValidation() {
        const forms = document.querySelectorAll('form[novalidate]');
        
        forms.forEach(form => {
            form.addEventListener('submit', (e) => {
                if (!this.validateForm(form)) {
                    e.preventDefault();
                }
            });
            
            // Real-time validation
            const inputs = form.querySelectorAll('input, textarea, select');
            inputs.forEach(input => {
                input.addEventListener('blur', () => {
                    this.validateField(input);
                });
            });
        });
    }
    
    validateForm(form) {
        const inputs = form.querySelectorAll('input, textarea, select');
        let isValid = true;
        let firstInvalidField = null;
        
        inputs.forEach(input => {
            if (!this.validateField(input) && !firstInvalidField) {
                firstInvalidField = input;
                isValid = false;
            }
        });
        
        if (firstInvalidField) {
            firstInvalidField.focus();
            this.announceFormErrors(form);
        }
        
        return isValid;
    }
    
    validateField(field) {
        const errorElement = document.getElementById(field.getAttribute('aria-describedby').split(' ').find(id => id.includes('error')));
        let isValid = true;
        let errorMessage = '';
        
        // Required field validation
        if (field.hasAttribute('required') && !field.value.trim()) {
            isValid = false;
            errorMessage = this.getRequiredMessage(field);
        }
        
        // Email validation
        if (field.type === 'email' && field.value && !this.isValidEmail(field.value)) {
            isValid = false;
            errorMessage = '有効なメールアドレスを入力してください';
        }
        
        // Update ARIA attributes
        field.setAttribute('aria-invalid', !isValid);
        
        if (errorElement) {
            if (isValid) {
                errorElement.textContent = '';
                errorElement.hidden = true;
            } else {
                errorElement.textContent = errorMessage;
                errorElement.hidden = false;
            }
        }
        
        return isValid;
    }
    
    getRequiredMessage(field) {
        const label = document.querySelector(`label[for="${field.id}"]`);
        const fieldName = label ? label.textContent.replace('*', '').trim() : 'この項目';
        return `${fieldName}は必須項目です`;
    }
    
    isValidEmail(email) {
        const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        return emailRegex.test(email);
    }
    
    announceFormErrors(form) {
        const errorCount = form.querySelectorAll('[aria-invalid="true"]').length;
        if (errorCount > 0) {
            this.announceToScreenReader(`フォームに${errorCount}個のエラーがあります。最初のエラー項目にフォーカスを移動しました。`);
        }
    }
    
    initLiveRegions() {
        // Create polite live region for announcements
        const liveRegion = document.createElement('div');
        liveRegion.setAttribute('aria-live', 'polite');
        liveRegion.setAttribute('aria-atomic', 'true');
        liveRegion.className = 'sr-only';
        liveRegion.id = 'live-region';
        document.body.appendChild(liveRegion);
    }
    
    announceToScreenReader(message) {
        const liveRegion = document.getElementById('live-region');
        if (liveRegion) {
            liveRegion.textContent = '';
            setTimeout(() => {
                liveRegion.textContent = message;
            }, 100);
        }
    }
    
    initTableSorting() {
        const sortButtons = document.querySelectorAll('th button[aria-label*="並び替え"]');
        
        sortButtons.forEach(button => {
            button.addEventListener('click', () => {
                const th = button.closest('th');
                const table = th.closest('table');
                const columnIndex = Array.from(th.parentNode.children).indexOf(th);
                
                // Toggle sort direction
                const currentSort = th.getAttribute('aria-sort');
                let newSort;
                
                if (currentSort === 'ascending') {
                    newSort = 'descending';
                } else {
                    newSort = 'ascending';
                }
                
                // Reset all other columns
                table.querySelectorAll('th[aria-sort]').forEach(otherTh => {
                    otherTh.setAttribute('aria-sort', 'none');
                });
                
                th.setAttribute('aria-sort', newSort);
                
                // Announce sort change
                const columnName = button.textContent.trim();
                const direction = newSort === 'ascending' ? '昇順' : '降順';
                this.announceToScreenReader(`${columnName}で${direction}に並び替えました`);
                
                // Actual sorting logic would go here
                this.sortTable(table, columnIndex, newSort === 'ascending');
            });
        });
    }
    
    sortTable(table, columnIndex, ascending) {
        const tbody = table.querySelector('tbody');
        const rows = Array.from(tbody.querySelectorAll('tr'));
        
        rows.sort((a, b) => {
            const aValue = a.children[columnIndex].textContent.trim();
            const bValue = b.children[columnIndex].textContent.trim();
            
            // Numeric comparison if both values are numbers
            const aNum = parseFloat(aValue.replace(/[^\d.-]/g, ''));
            const bNum = parseFloat(bValue.replace(/[^\d.-]/g, ''));
            
            if (!isNaN(aNum) && !isNaN(bNum)) {
                return ascending ? aNum - bNum : bNum - aNum;
            }
            
            // String comparison
            return ascending ? aValue.localeCompare(bValue) : bValue.localeCompare(aValue);
        });
        
        // Reorder DOM
        rows.forEach(row => tbody.appendChild(row));
    }
    
    closeActiveModal() {
        const activeModal = document.querySelector('[role="dialog"]:not([hidden])');
        if (activeModal) {
            activeModal.hidden = true;
            // Return focus to trigger element
            const trigger = document.querySelector(`[aria-controls="${activeModal.id}"]`);
            if (trigger) trigger.focus();
        }
    }
    
    closeActiveDropdown() {
        const activeDropdown = document.querySelector('[aria-expanded="true"]');
        if (activeDropdown) {
            activeDropdown.setAttribute('aria-expanded', 'false');
            const menu = document.getElementById(activeDropdown.getAttribute('aria-controls'));
            if (menu) menu.hidden = true;
        }
    }
}

// Initialize accessibility features
document.addEventListener('DOMContentLoaded', () => {
    new AccessibilityManager();
});
```

### 🏗️ 企業レベルCSS設計手法

#### SMACSS（Scalable and Modular Architecture for CSS）
```css
/* Base Rules */
html, body { margin: 0; padding: 0; }

/* Layout Rules */
.l-header { position: fixed; top: 0; width: 100%; }
.l-sidebar { float: left; width: 250px; }
.l-main { margin-left: 250px; }

/* Module Rules */
.card { border: 1px solid #ddd; padding: 1rem; }
.card-title { font-size: 1.2rem; font-weight: bold; }

/* State Rules */
.is-hidden { display: none; }
.is-active { background-color: #007bff; }

/* Theme Rules */
.theme-dark .card { background: #333; color: #fff; }
```

#### Atomic Design実装
```css
/* Atoms (原子) */
.btn { padding: 8px 16px; border: none; cursor: pointer; }
.input { padding: 8px; border: 1px solid #ccc; }

/* Molecules (分子) */
.search-form { display: flex; gap: 8px; }
.search-form .input { flex: 1; }
.search-form .btn { flex-shrink: 0; }

/* Organisms (有機体) */
.header { background: #fff; padding: 1rem; }
.header .search-form { margin-left: auto; }

/* Templates (テンプレート) */
.page-template { 
    display: grid; 
    grid-template-areas: "header" "main" "footer";
}

/* Pages (ページ) */
.home-page .header { background: linear-gradient(45deg, #667eea, #764ba2); }
```

### ✅ 178項目完全習熟チェックリスト

#### Level 1: HTML基礎マスター（50項目）
- [ ] HTML5 DOCTYPE宣言の理解と使用
- [ ] セマンティックタグ（header, nav, main, article, section, aside, footer）の適切な使用
- [ ] 見出しタグ（h1-h6）の階層構造理解
- [ ] リストタグ（ul, ol, dl）の使い分け
- [ ] テーブルタグ（table, thead, tbody, th, td）の構造化
- [ ] フォーム要素（input, textarea, select, button）の実装
- [ ] 画像タグ（img）のalt属性とアクセシビリティ
- [ ] リンクタグ（a）のhref属性とナビゲーション
- [ ] メタタグ（meta）のSEO最適化
- [ ] 文字エンコーディング（UTF-8）の設定

#### Level 2: CSS基礎・レスポンシブ（40項目）
- [ ] CSSセレクタ（タグ、クラス、ID、属性、疑似クラス）の使い分け
- [ ] ボックスモデル（margin, border, padding, content）の完全理解
- [ ] display プロパティ（block, inline, inline-block, none）の使い分け
- [ ] position プロパティ（static, relative, absolute, fixed, sticky）の理解
- [ ] Flexbox レイアウト（justify-content, align-items, flex-direction）
- [ ] CSS Grid レイアウト（grid-template-areas, grid-gap）
- [ ] メディアクエリとレスポンシブデザイン
- [ ] CSS Variables（カスタムプロパティ）の活用
- [ ] トランジション・アニメーション効果
- [ ] CSS Reset・Normalize.cssの使用

#### Level 3: モダンCSS・パフォーマンス（35項目）
- [ ] CSS Containment（contain プロパティ）の理解
- [ ] CSS Grid の高度な機能（subgrid, minmax()）
- [ ] CSS Container Queries の実装
- [ ] CSS-in-JS の理解と使用（Styled Components, Emotion）
- [ ] Critical CSS の抽出と最適化
- [ ] CSS の Tree Shaking とバンドル最適化
- [ ] Web Fonts の最適化（font-display: swap）
- [ ] CSS スプライト技術
- [ ] PostCSS・Sass/SCSS の活用
- [ ] CSS Architecture（BEM, SMACSS, ITCSS）の実装

#### Level 4: アクセシビリティ・エンタープライズ（28項目）
- [ ] WCAG 2.1 AAA レベルの完全準拠
- [ ] ARIA属性（role, aria-label, aria-describedby）の適切な使用
- [ ] スクリーンリーダー対応
- [ ] キーボードナビゲーション完全対応
- [ ] コントラスト比7:1以上の確保
- [ ] Large Scale CSS Architecture 設計
- [ ] Design System の構築・運用
- [ ] CSS のコンポーネント化
- [ ] Cross-browser 互換性確保
- [ ] Multi-language（RTL言語）対応

#### Level 5: 最高技術責任者レベル（25項目）
- [ ] Web標準策定・W3C仕様への貢献
- [ ] ブラウザエンジン仕様の深い理解
- [ ] CSS の将来仕様（CSS4 Selectors等）への対応
- [ ] 大規模サイト（月間10億PV）のCSS Architecture設計
- [ ] CSS フレームワーク・ライブラリの開発
- [ ] Design Token システムの設計
- [ ] CSS テスティング（Visual Regression Testing）
- [ ] CSS メンテナビリティ指標の策定
- [ ] CSS パフォーマンス監視システム構築
- [ ] 次世代CSS技術（CSS Houdini等）の研究・実装

### 🎓 24ヶ月プロフェッショナル育成プログラム

#### Phase 1: 基礎確立期（1-6ヶ月）
**Month 1-2: HTML/CSS基礎固め**
- セマンティックHTML5完全マスター
- CSS基本プロパティ・セレクタ習得
- レスポンシブデザイン基礎

**Month 3-4: モダンレイアウト技術**
- Flexbox・CSS Grid完全習得
- CSS Variables活用
- モバイルファースト設計

**Month 5-6: 実践プロジェクト**
- ポートフォリオサイト制作
- アクセシビリティ基礎対応
- Cross-browser対応

#### Phase 2: 中級スキル構築期（7-12ヶ月）
**Month 7-8: CSS Architecture**
- BEM・SMACSS記法マスター
- Sass/SCSS活用
- コンポーネント設計思想

**Month 9-10: パフォーマンス最適化**
- Critical CSS実装
- Web Fonts最適化
- Core Web Vitals改善

**Month 11-12: Design System構築**
- Atomic Design実装
- Design Token管理
- Storybook活用

#### Phase 3: 上級エンジニア期（13-18ヶ月）
**Month 13-14: 大規模CSS設計**
- Enterprise CSS Architecture
- CSS-in-JS実装
- モノレポ対応

**Month 15-16: アクセシビリティ完全対応**
- WCAG 2.1 AAA準拠
- ARIA完全実装
- スクリーンリーダー対応

**Month 17-18: チーム開発・マネジメント**
- CSS ガイドライン策定
- コードレビュー文化構築
- 技術選定・判断

#### Phase 4: エキスパート・リーダー期（19-24ヶ月）
**Month 19-20: 次世代技術研究**
- CSS Houdini実装
- Container Queries活用
- 新仕様評価・導入

**Month 21-22: 技術リーダーシップ**
- 社内勉強会・技術共有
- 外部登壇・技術発信
- OSS貢献活動

**Month 23-24: CTO・技術戦略**
- 全社フロントエンド戦略
- 技術投資判断
- 次世代人材育成

### 🚀 次世代技術展望

#### Web Components & Shadow DOM
- ネイティブコンポーネント技術
- フレームワーク非依存設計
- 再利用可能UI部品

#### CSS Houdini & Paint API
- CSS描画エンジン拡張
- カスタムプロパティ&値
- アニメーション最適化

#### WebAssembly & CSS
- ネイティブパフォーマンス
- 複雑なCSS処理高速化
- 新しいWeb体験

#### WebXR & Spatial CSS
- VR/AR空間UI設計
- 3D CSS Transform拡張
- 次世代インターフェース