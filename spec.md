# FORM — design-apparel-minimal Spec

**Status:** Approved  
**Author:** torifo  
**Created:** 2026-05-18  
**Updated:** 2026-05-18

---

## 1. Overview

### Problem Statement
ミニマルファッション志向の若者（20–26歳・デザイン系）は、「ミニマル」を謳うブランドサイトでも装飾が多すぎる・余白が怖くて詰め込まれている・アニメーションが過剰と感じており、本当の「引き算」を体現したサイトがない。

### Goal
建築事務所のポートフォリオとクワイエットラグジュアリーを美学的出発点とした架空ブランド「FORM」のランディングページを実装する。極端な余白・1pxボーダーのみ・アニメーション最小限という制約の中で美しさを生む技術を検証する。

### Non-Goals
- カート・決済機能
- CMS連携
- ダークモード（白のみに集中）

### Background
- FORMは本デザイン研究のために作成した架空ブランドであり、実在のブランド・店舗・商品ではない
- `design-apparel-minimal` リポジトリ、`design-apparel-minimal.riumu.net` 独自ドメイン予定
- 同シリーズで最もアニメーション量が少ない。制約が設計の核

---

## 2. User Stories

| ID | Persona | Want to | So that |
|----|---------|---------|---------|
| US-01 | minimal（20–26歳・設計思考） | ページを開いた瞬間に「余白の美学」を感じたい | ブランドの哲学が即座に伝わる |
| US-02 | minimal | 商品のシルエット・素材情報を静かに確認したい | 不要な視覚ノイズなしに判断できる |
| US-03 | minimal | ブランドのスタンスを短く・明確に読みたい | 長い説明なしに共感できる |
| US-04 | minimal | どのデバイスでも同じ緊張感ある体験を得たい | 空間の質が保たれている |

### Acceptance Criteria (EARS notation)

**US-01: 余白の体験**
- WHEN ページが読み込まれた THEN コンテンツがフェードイン（0.6sのみ）で現れ、余白が全体の60%以上を占める
- WHEN ヒーローを見た THEN 装飾なし・画像なし・タイポグラフィのみのレイアウトが確認できる
- WHEN ページを眺めた THEN 1px水平ボーダー以外の装飾要素がない

**US-02: 商品の静的ブラウズ**
- WHEN ユーザーが商品グリッドに到達した THEN 4枚が厳格な等幅グリッドで整列している
- WHEN 商品カードにホバーした THEN scale(1.02) のごく微小なズームのみ（他効果なし）
- WHEN 商品名を見た THEN タイポグラフィのみで価格と名前が表示される

**US-03: ブランドスタンスの確認**
- WHEN 黒背景反転セクションに到達した THEN 白テキストで短いステートメントが2段組で読める
- WHEN 読んだ THEN 50語以下で完結している

**US-04: 全デバイス対応**
- WHEN 375px幅で閲覧した THEN グリッドが2カラムに変わり横スクロールが発生しない
- WHEN デスクトップで閲覧した THEN padding/margin がモバイルの1.5倍以上になる

---

## 3. Functional Requirements

| ID | Requirement | Priority | Notes |
|----|-------------|----------|-------|
| FR-01 | テキストのみヒーロー（画像なし） | P0 | 超大型Bebas Neue |
| FR-02 | 厳格4カラム商品グリッド | P0 | グリッドブレイクなし |
| FR-03 | 黒背景反転セクション（Philosophy） | P0 | 白テキスト2段組 |
| FR-04 | 素材リストセクション（テキストのみ） | P1 | 素材名・産地・特性 |
| FR-05 | 1商品フィーチャーセクション（全幅） | P1 | 詳細テキスト付き |
| FR-06 | フッター（1pxボーダー+最小リンク） | P1 | |
| FR-07 | フェードインのみのアニメーション | P0 | translateY なし |
| FR-08 | ナビゲーション（テキストのみ、1px下ボーダー） | P0 | アイコン不使用 |
| FR-09 | グレースケール商品画像 | P0 | grayscale(100%) |
| FR-10 | モバイルファースト対応（375px基準） | P0 | |
| FR-11 | テクスチャ・グレイン・カーソムカーソル不使用 | P0 | 制約として明記 |

---

## 4. Architecture

### Page Structure

```
index.html
├── <nav>                          # テキストロゴ、1px下ボーダー
├── <section class="hero">         # テキストのみ、巨大Bebas Neue + 余白
├── <section class="collection">   # 厳格4カラムグリッド
├── <section class="philosophy">   # 黒背景反転、白テキスト2段組
├── <section class="materials">    # テキストリスト（素材表）
├── <section class="feature">      # 1商品フィーチャー（全幅）
└── <footer>                       # 1px上ボーダー + コピー + リンク
```

### Component Responsibilities

| Component | Responsibility |
|-----------|---------------|
| Nav | 存在を主張しない。ブランド名とページリンクのみ |
| Hero | タイポグラフィのみで最大のインパクト。余白がコンテンツ |
| Collection | 整列と均等で信頼感を出す。発見感より安心感 |
| Philosophy | 唯一のドラマ。白↔黒の反転で空間のアクセント |
| Materials | 透明性の証明。装飾不要、テキストのみ |
| Feature | 一点集中。商品詳細への想像力を喚起 |

### Key Design Decisions

| Decision | Chosen | Rationale | Rejected alternatives |
|----------|--------|-----------|----------------------|
| テーマ | ライトモード（ほぼ白） | ミニマルの哲学そのもの。余白=空間 | ダークモード（ARCHと被る） |
| Display font | Bebas Neue | 大文字専用コンデンスで圧倒的な存在感。他3サイトと差別化 | Helvetica Neue（ライセンス）、Futura（古い） |
| Body font | Outfit | 幾何学的・現代的・クリーン。Bebasとの対比 | DM Sans（他サイトで使用）、Inter（汎用すぎ） |
| グリッド | 厳格等幅（ブレイクなし） | 建築的整列。グリッドブレイクは「賑やかさ」につながる | 非対称（ARCHと同質化） |
| アニメーション | opacity 0.6sのみ | translateYは「動き」を感じさせすぎる。現れるだけで十分 | slideUp（FORMには過剰） |
| カーソル | デフォルト | 装飾的カーソルは余計なもの。哲学と一致 | カスタム（LUEURやARCHに任せる） |
| Grain/Texture | 使用しない | ノイズは「引き算」の思想に反する | 薄いGrain（それでも余計） |
| Hero | 画像なし・テキストのみ | 写真は情報過多。タイポグラフィだけで世界観を出す | 写真あり（他3サイトと同質化） |

---

## 5. Design System

### Color Palette
```css
--bg:           #FAFAFA;   /* ピュアに近いホワイト */
--bg-sub:       #F4F4F4;   /* わずかに沈んだ白 */
--bg-dark:      #111111;   /* 反転セクション用 */
--fg:           #111111;   /* ほぼ黒 */
--fg-muted:     #888888;   /* ミッドグレー */
--border:       #E0E0E0;   /* 1pxボーダー専用 */
--accent:       #D4C4A8;   /* ウォームサンド（唯一の暖色） */
--accent-dark:  #8A7A60;   /* 深サンド */
```

### Typography
```css
--font-display: 'Bebas Neue', sans-serif;        /* 大文字専用 */
--font-body:    'Outfit', 'Noto Sans JP', sans-serif;
```
- Google Fonts CDN から読み込み
- Bebas Neue は見出し・ロゴ・全大文字テキストのみ
- 日本語は Noto Sans JP weight:300 で細め・上品に

### Spacing & Motion
```css
--ease: cubic-bezier(0.4, 0, 0.2, 1);  /* 精密・機械的 */
/* section padding: 8rem 4rem (desktop), 5rem 1.5rem (mobile) */
/* reveal: opacity 0→1, 0.6s, no transform */
/* product hover: scale(1.02), 0.4s */
```

---

## 9. Testing Strategy (Visual QA)

| Layer | Scenarios |
|-------|-----------|
| Desktop (1280px) | 余白60%以上確認、1pxボーダーのみ、厳格グリッド整列 |
| Mobile (375px) | 2カラム化、余白縮小だが圧迫感なし、横スクロールなし |
| アニメーション | opacity fadeのみ確認、translateY が発生していないこと |
| ホバー | scale(1.02)のみ。他の視覚効果が発生しないこと |
| フォント | Bebas Neue（全大文字）・Outfit 正常適用確認 |
| 制約確認 | grain/texture・カスタムカーソル・ローダーが存在しないこと |

---

## 10. Implementation Notes

- **Hero**: テキストのみ。`font-size: clamp(6rem, 20vw, 18rem)` で画面幅に対してスケール
- **グリッド**: `grid-template-columns: repeat(4, 1fr)` に固定。レスポンシブで `repeat(2, 1fr)`
- **黒反転セクション**: `background: var(--bg-dark); color: #FAFAFA` — CSSのみで完結
- **素材リスト**: `<dl>` タグ（dt=素材名, dd=産地・特性）で実装
- **グレースケール**: `filter: grayscale(100%) contrast(1.1)` を img に適用
- **reveal**: `IntersectionObserver` で `.reveal` に `.in` 付与。`transition: opacity 0.6s var(--ease)` のみ
- **装飾排除の自動チェック**: 実装後 CSS 内で `cursor`, `grain`, `noise`, `transform: translate` の意図しない使用がないか確認

---

## 11. Open Questions

| # | Question | Owner | Due | Status |
|---|----------|-------|-----|--------|
| 1 | Hero のテキストは「FORM」のみか、サブテキストを加えるか | torifo | 実装時 | Open |
| 2 | Philosophyセクションのステートメント文言（日本語か英語か） | torifo | 実装時 | Open |
| 3 | `design-apparel-minimal.riumu.net` のDNS設定タイミング | torifo | 後日 | Open |

---

## References

- [spec.md（ナビゲーター）](../spec.md)
- Font: [Bebas Neue](https://fonts.google.com/specimen/Bebas+Neue), [Outfit](https://fonts.google.com/specimen/Outfit)
- Inspiration: [COS](https://www.cosstores.com), [Auralee](https://auralee.jp), [COMOLI](https://comoli.jp)
