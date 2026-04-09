# GitHub Pages 個人サイト 設計ドキュメント

**作成日:** 2026-04-09  
**目的:** 研究者・就活兼用のアカデミックポートフォリオサイトをGitHub Pagesで構築する

---

## 概要

自己紹介（Bio）、論文リスト（Publications）、研究紹介（Research）を掲載する日英バイリンガルの個人サイト。al-folioテンプレートをベースに構築し、GitHub Actionsで自動デプロイする。

---

## 技術スタック

- **フレームワーク:** [al-folio](https://github.com/alshedivat/al-folio)（Jekyll）
- **ホスティング:** GitHub Pages（`username.github.io`）
- **デプロイ:** GitHub Actions（mainブランチへのpushで自動ビルド → gh-pagesブランチへ公開）
- **言語:** 日英バイリンガル（EN / JP 切り替え）

---

## ディレクトリ構成

```
github.io/
├── _config.yml              # サイト全体設定（名前・SNS・著者名・言語）
├── _data/
│   └── publications.yml     # 論文リスト（YAMLで管理）
├── _pages/
│   ├── about.md             # Bio（トップページ）
│   ├── publications.md      # 論文一覧ページ
│   └── research.md          # 研究テーマページ
├── assets/
│   ├── img/                 # プロフィール画像等
│   └── pdf/                 # CV等のPDF
└── docs/
    └── superpowers/
        └── specs/           # 設計ドキュメント
```

---

## 各ページの仕様

### About（トップページ）
- プロフィール写真
- 名前・所属・研究分野（短いラベル程度）
- SNSリンク（GitHub、Google Scholar、メール等）
- CV ダウンロードボタン（PDF）
- 言語切り替えボタン（EN / JP）
- 自己紹介文は初期実装では省略

### Publications
- `_data/publications.yml` で論文データを管理
- YAML形式（BibTeX不要）:
  ```yaml
  - title: "論文タイトル"
    authors: "著者名1, 著者名2, ..."
    venue: "会議名 / ジャーナル名"
    year: 2024
    url: "https://arxiv.org/..."   # なければ省略可
  ```
- `_config.yml` に自分の名前を設定 → 著者リストで自動ボールド表示
- URLがあればタイトルがクリッカブルリンクになる
- 年度降順で表示

### Research
- 研究テーマを2〜4つ掲載
- 各テーマの構成:
  - タイトル
  - 概要文（2〜3段落）
  - 関連論文へのリンク（任意）
  - 図・画像（任意）

---

## 日英バイリンガル対応

- `_config.yml` でデフォルト言語を英語（`lang: en`）に設定
- 各ページのフロントマターで言語を指定
- al-folioの多言語サポート機能を活用
- About、Publications、ResearchそれぞれにEN/JP版を用意

---

## デプロイ・運用フロー

### 初回セットアップ
1. al-folio をテンプレートとしてGitHubにリポジトリ作成（`username.github.io`）
2. `_config.yml` に名前・所属・SNSリンク・著者名を記入
3. GitHub Pages の設定で `gh-pages` ブランチを公開元に指定
4. GitHub Actions が自動でビルド・デプロイ

### 日常的なコンテンツ更新

| やりたいこと | 操作 |
|---|---|
| 論文を追加 | `_data/publications.yml` に数行追記してpush |
| 研究テーマを更新 | `_pages/research.md` を編集してpush |
| プロフィール写真変更 | `assets/img/` に画像を置いて `_config.yml` のパスを変更 |
| CV更新 | `assets/pdf/` にPDFを置き換え |

pushするだけで自動ビルド・公開される。手元にビルド環境は不要。

---

## スコープ外（今回は実装しない）

- ブログ / News機能
- コメント機能
- アクセス解析
- お問い合わせフォーム
