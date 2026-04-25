# blog-kenshintanaka-jp

田中けんしんの個人ブログのソース。`kenshintanaka.jp` で公開。

## 設計方針

「編集は最新技術、表示は枯れた技術」。

- 編集は Claude Code に任せる
- 生成物は素の HTML + CSS のみ
- ビルドツール・JS フレームワーク・Web フォント・JS は使わない
- 100 年後でも開けるファイルだけを残す

## 構成

- ソース管理: GitHub（このリポジトリ）
- ホスティング: Cloudflare Pages（メイン）+ GitHub Pages（ミラー）
- ビルド: なし。HTML を直接コミット

## ディレクトリ

```
.
├── index.html         トップ。記事一覧
├── style.css          サイト全体で 1 枚のみ
└── posts/
    └── YYYY-MM-DD-slug.html
```

ヘッダー・フッターなど共通要素は、include 機構を使わず各 HTML に
ハードコピーする。変更時は Claude Code が一括編集する。

## ライセンス

本文・コード含め [CC BY 4.0](./LICENSE)。
