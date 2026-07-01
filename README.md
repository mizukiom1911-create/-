# nagaoka-mugicha-lp — Shopify Dawnテーマ（麦茶LP統合版）

ブランチ `nagaoka-mugicha-lp`：フラットに置かれていた161ファイルを、
Shopifyの正しいテーマ階層に再配置し、麦茶LPを統合したものです。

## 構成

```
config/     settings_schema.json, settings_data.json
layout/     theme.liquid（← theme.css読み込み1行を追加済み）, password.liquid
locales/    49ファイル（多言語）
sections/   68ファイル（Dawn標準 + 麦茶カスタム16 + header/footer-group）
snippets/   cart-drawer, quick-order-list, badge-shubun
templates/  21ファイル（index.json 等 + page.mugicha.json + customers/）
assets/     theme.css
_design-system/  参考資料・プレビュー（テーマ本体ではない）
```

麦茶LP: `templates/page.mugicha.json`（全16セクション、すべて sections/ に存在確認済み）

## ⚠️ 未解決の重大な欠落：assets/ のJS・CSSが無い

このDawnエクスポートには **JavaScript も base.css も画像も含まれていません**
（assets/ にあるのは追加した `theme.css` のみ）。

`layout/theme.liquid` は Dawn標準どおり次を読み込もうとしますが、実体がありません：
`constants.js` `pubsub.js` `global.js` `cart-disclosure-modal.js`
`details-disclosure.js` `details-modal.js` `search-form.js` `animations.js`
`base.css` … など多数。

→ このままGitHub連携でShopifyに同期すると、**基本スタイル・カート・
　検索・アコーディオン等のインタラクションが動かない**壊れたテーマになります。

### 対処（どちらか）
- **推奨**：Shopifyストア側で **Dawnを一度インストール**（テーマストアから追加）
  してGitHub連携し、そこへこの `sections/ templates/ snippets/ layout/theme.liquid
  assets/theme.css` を**マージ**する。Dawn本体のassetsはストア側にある状態にする。
- または、Dawnの公式 assets/ 一式（375ファイル版の assets/）をこのリポジトリに
  補充してから同期する。

麦茶ヒーローのAjaxカート追加は、Dawnの `global.js` 等が存在する環境でのみ動作します。
