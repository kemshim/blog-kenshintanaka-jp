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
├── about.html         自己紹介
├── style.css          サイト全体で 1 枚のみ
├── rss.xml            RSS フィード
├── sitemap.xml        サイトマップ
├── robots.txt
├── .nojekyll          GitHub Pages で Jekyll 処理を無効化
├── .github/workflows/
│   └── archive.yml    archive.org / archive.today への自動送信
└── posts/
    └── YYYY-MM-DD-slug.html
```

ヘッダー・フッターなど共通要素は、include 機構を使わず各 HTML に
ハードコピーする。変更時は Claude Code が一括編集する。

内部リンクはすべて**相対パス**で書く。こうしておくと、ルートで配信される
Cloudflare Pages (`kenshintanaka.jp`) でも、サブパスで配信される
GitHub Pages ミラー（`kemshim.github.io/blog-kenshintanaka-jp/`）でも
同じファイルで動く。canonical URL だけは絶対 URL。

## 記事を追加するとき

ビルドを持たないので、記事 1 本を足すときに更新する箇所が複数ある。
Claude Code に頼むと一括でやってくれる。

1. `posts/YYYY-MM-DD-slug.html` を作成
2. `index.html` の記事一覧に 1 行追加（新しい順）
3. `rss.xml` に `<item>` を追加
4. `sitemap.xml` に `<url>` を追加

## ホスティング

### Cloudflare Pages（メイン）

- `kenshintanaka.jp` に割り当て
- GitHub 連携で `main` ブランチを自動デプロイ
- ビルドコマンドは空。出力ディレクトリはリポジトリ直下

### GitHub Pages（ミラー）

- `https://kemshim.github.io/blog-kenshintanaka-jp/` で公開
- リポジトリ Settings → Pages → Source: "Deploy from a branch" → `main` / `/ (root)`
- `.nojekyll` を置いて Jekyll 処理をバイパス
- 相対パスで組んであるので、サブパス配信でも壊れない
- canonical は常に `kenshintanaka.jp/...`。検索エンジンには本家を指す

## アーカイブ

`main` への push 時と毎月 1 日に `.github/workflows/archive.yml` が走り、
以下へスナップショットを送る。

- [web.archive.org (Wayback Machine)](https://web.archive.org/) Save Page Now
- [archive.today](https://archive.today/)

どちらも未認証の公開エンドポイントを叩くだけ。レート制限に引っかかっても
次回リランで補える作りにしてある。

手動実行は Actions タブの "archive" から `Run workflow`。

国立国会図書館 WARP は申請ベースなので、落ち着いたら検討する。

## ライセンス

本文・コード含め [CC BY 4.0](./LICENSE)。
