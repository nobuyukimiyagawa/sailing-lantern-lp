# Sailing Lantern — Article LP build

> **⚠ INTERNAL PREVIEW — UNREVIEWED**
> This is an unreviewed draft for stakeholder review. **LoveFrom approval pending.**
> Press quotes are pending commercial-use clearance. Do not redistribute.
> `noindex,nofollow` is set; please do not link publicly.

US/EU 向け記事 LP の静的実装。`36_article_lp_us_eu_draft.md` のドラフトをそのまま HTML/CSS 化したもの。

## ディレクトリ構成

```
lp_build/
├── index.html              # 本体 1 ファイル
├── assets/
│   ├── css/styles.css      # editorial デザイン、design tokens
│   └── img/                # 画像（現状は SVG プレースホルダ）
└── README.md               # 本ファイル
```

## ローカル確認

```sh
cd /Users/miyagawanobuyuki/Desktop/LoveFrom/lp_build
python3 -m http.server 8765
# → http://localhost:8765/
```

直接 `open index.html` でも閲覧可（`file://` でも動作するように相対パスで構築）。

## 画像差し替え

現状はすべて `assets/img/*.svg` のプレースホルダ。HTML 側は `.jpg` 名で参照しており、`.jpg` は SVG への symlink。本番差し替え時は **同じファイル名で実画像を置けば置換される**。

| ファイル | 用途 | アスペクト比 | サイズ |
|---|---|---|---|
| `hero@{800,1200,1800}.jpg`         | Hero（暗背景の object）            | 4:5  | 800 / 1200 / 1800w |
| `object-three-quarter@{800,1200}.jpg` | § 1 Object（点灯 3/4 アングル）  | 4:3  | 800 / 1200w |
| `dial-detail@{800,1200}.jpg`       | § 2 Origin（dial close-up、手の甲のみ可）| 4:3 | 800 / 1200w |
| `materials@{800,1200}.jpg`         | § 3 Materials（素材 3 連 or stack）| 3:2 | 800 / 1200w |
| `lit-in-dark@{800,1200}.jpg`       | § 5 闇に浮かぶ object             | 4:3 | 800 / 1200w |
| `patina@{800,1200}.jpg`            | § 6 経年想起 / brass close-up     | 4:3 | 800 / 1200w |
| `object-side@{800,1200}.jpg`       | § 7 静かな side profile           | 4:3 | 800 / 1200w |

**形式推奨**：WebP（fallback JPEG）。AVIF も可。差し替え時は HTML の `srcset` を `.webp` 形式に書き換えると最適。

## ブリーフ §10 制約遵守チェック

- ✅ Ferrari 言及なし
- ✅ 全画像 object-first、人物の顔・身体なし（手の甲のみ可）
- ✅ "Stunning" / "Finally" / "At last" 不使用
- ✅ Urgency 表現なし（"1,000 pieces, worldwide" は事実提示のみ）
- ✅ 新規 LoveFrom コメントなし。引用はすべて既公開（Boat Int / Wallpaper\* / Dezeen / 9to5Mac / BALMUDA）
- ✅ Confirmation トーン徹底

## SEO / 構造化データ

- `Schema.org Product + Review` JSON-LD を `<head>` 内に埋め込み済
- `Review` は Dezeen / Wallpaper\* (Jony Ive 引用) / Boat International (Jony Ive 引用) の 3 件
- `hreflang` を EN / FR / DE / IT + `x-default` で設定（多言語版未実装でも本番 deploy 時に有効）
- OG / Twitter Card 設定済（OG image は `hero@1200.jpg`）

## パフォーマンス目標

- LCP < 2.5s（Hero 画像 `fetchpriority="high"` + `<link rel="preload">` で前倒し）
- CLS < 0.1（全 `<img>` に `width/height` を明示）
- INP < 200ms（JS ゼロ）

実装方針：
- JS なし、フォントは Google Fonts CDN（preconnect 済）
- `prefers-reduced-motion: no-preference` 時のみ `animation-timeline: view()` で fade-in（モダンブラウザのみ、フォールバックなし）

## 配置・運用上の判断事項（要決定）

draft #36 §6 と同じ。**実装着手前に下記を確定する必要あり**：

1. **URL**：`/lovefrom-balmuda/press/` か `/sailing-lantern/story/`
2. **既存 LP との関係**：既存 LP からのリンク追加？
3. **多言語展開の優先順位**：EN → FR / DE / IT
4. **引用の商用利用許諾**：Dezeen / Wallpaper\* / Boat International / 9to5Mac へ確認
5. **LoveFrom（Sarah）承認**：本文・引用・design すべて
6. **配信開始日**：5/5 / 5/20 / 6 月以降
7. **制作費見積**：別予算 pitch 25-30 万円相当

## デプロイ前 TODO

- [ ] 本物の画像 8 種を `assets/img/` に配置（WebP 推奨）
- [ ] フォントを self-host or 配信先 CDN に変更（Google Fonts のままで可）
- [ ] `<link rel="canonical">` のドメインを確定して書き換え
- [ ] OG image の URL を本番ドメインに書き換え
- [ ] Schema.org JSON-LD の `image` URL を本番に書き換え
- [ ] GTM / Pixel タグ挿入（既設 GTM-WDP2QX を再利用、`balmuda.com` 既存 GTM 環境に追従）
- [ ] FR / DE / IT 翻訳版作成（`/fr/`, `/de/`, `/it/` 配下）
- [ ] 法務レビュー（引用、価格表記、地域別法定表記）

## 引用元と日付（出典明記）

| 引用 | 出典 | 日付 |
|---|---|---|
| "Warmth and emotional resonance of a live flame." | Dezeen | 2025-09-30 |
| "If you hold it, it just feels right..." (Jony Ive) | Wallpaper\* | 2025-09-27 |
| "I would have just bought a lantern for my yacht..." (Jony Ive) | Boat International | 2025-09 |
| "The value comes from the care..." (Jony Ive) | Boat International | 2025-09 |
| "It has been a considerable honour..." (Jony Ive) | 9to5Mac | 2025-09 |
| "The level of precision and polish..." (Gen Terao) | Wallpaper\* | 2025-09-27 |
| "This collaboration is built on mutual respect..." (Gen Terao) | BALMUDA 公式 | 2025-09 |

すべて draft #14 (`14_press_archive_details.md`) で確認済。

---

**作成**：2026-04-25
**ベースドラフト**：`36_article_lp_us_eu_draft.md`
