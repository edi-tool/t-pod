# 開発の記録（progress.md）

このPWAの開発経緯・現状・残課題のログです。新しい作業を行ったら追記してください。

## 現状サマリ
- **マルチイベント対応のタイムテーブル PWA が完成**し、`main` にマージ済み。
- 公開URL（GitHub Pages 有効化後）:
  - イベント一覧: `https://<user>.github.io/t-pod/`
  - 個別イベント: `https://<user>.github.io/t-pod/?id=2026-zensanken-37`
- 現在登録されているイベント: **第37回 全国算数授業研究大会**（`events/2026-zensanken-37.json`）。

## マイルストーン（PR単位）

### PR #1 — 初期PWA構築（merged）
- ディレクトリ構成・レスポンシブUI（Tailwind CDN）・JSONパース・Service Worker を実装。
- ボトムナビ3タブ（プログラム / 資料 / 書籍）、Sticky ヘッダー、FAB、各種モーダル。
- Service Worker による Network First キャッシュ（オフライン対応）。
- サンプル `data.json`（架空イベント）を同梱。

### PR #2 — 実データ反映 + UI 汎用化（merged）
- 第37回 全国算数授業研究大会の実データ（テーマ・会期・会場・1〜2日目の全プログラム・登壇者・2日目ワークショップ案内）を反映。
- タイムテーブルを **room×time グリッド → 縦型タイムライン**（時間 → 内容 → 詳細）に変更。全体会中心＋一部 A〜D 会場の分科会に対応。
- `index.html` から固有文言を排除し、タイトル等を JSON から動的反映する**汎用シェル化**。`manifest.json` も汎用名に。

### PR #3 — マルチイベント対応（merged）
- `?id=<id>` クエリパラメータ方式を導入。`events/<id>.json` にイベント詳細を集約（旧 `data.json` を `events/2026-zensanken-37.json` に移設）。
- `events.json`（一覧インデックス）を追加。`?id` 無しのルートURLは一覧表示。
- `sw.js` の precache を `data.json` → `events.json` に変更、`CACHE_VERSION` を v3 に。
- 目的: **過去のチラシ（QR）からアクセスしても当時の情報が永続的に見られる**ようにする。

### ドキュメント整備
- `CLAUDE.md`（プロジェクトガイド）と `progress.md`（本ファイル）を追加。

## 残課題 / TODO
- [ ] **GitHub Pages の有効化**（Settings → Pages → main / root）。
- [ ] **判読困難だった氏名・所属の確認**（画像から転記したもの）:
  - 2日目ワークショップ案内の登壇者・所属（例: 山本良一／島根・雲南市立木次小、髙木美和／福岡・赤村立赤小 など）。
  - シンポジウム討論者「久保田健功」の表記。
  - 修正は `events/2026-zensanken-37.json` の該当箇所のみ。
- [ ] （任意）QRコード生成の仕組み。
- [ ] （任意）書籍データの拡充（実際の販売書籍リスト）。
- [ ] （任意）会場マップ画像（`assets/venue-map.svg`）の本番図面への差し替え。
- [ ] （任意）PWAアイコン（`assets/icon.svg`）の本番デザインへの差し替え。
