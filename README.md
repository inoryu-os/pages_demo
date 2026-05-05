# Team Knowledge Demo

## URL
https://github.com/inoryu-os/pages_demo.git

Zensical と GitHub Pages で、チーム向けナレッジドキュメントを公開するためのデモリポジトリです。

Markdown のソースは `main` ブランチに置き、Zensical で HTML にビルドした成果物を `gh-pages` ブランチへデプロイします。Pull Request ごとに `/pr-<PR番号>/` のプレビューURLも生成します。

## ディレクトリ構成

```text
.
├── .github/
│   └── workflows/
│       ├── deploy-production.yml
│       └── pr-preview.yml
├── docs/
│   ├── index.md
│   ├── onboarding.md
│   └── operations.md
├── .gitignore
├── README.md
└── zensical.toml
```

## Zensical 設定

`zensical.toml` では、Markdown ソースとビルド成果物を次のように設定しています。

```toml
[project]
docs_dir = "docs"
site_dir = "site"
```

GitHub Actions でも `zensical build --clean` の出力先である `./site` を `peaceiris/actions-gh-pages` の `publish_dir` に指定しています。

## セットアップ方法

1. このディレクトリをGitHubリポジトリとして作成します。

   ```bash
   git init
   git branch -M main
   git add .
   git commit -m "Create knowledge site demo"
   git remote add origin git@github.com:<org-or-user>/<repo>.git
   git push -u origin main
   ```

2. `zensical.toml` の `site_url` を実際のURLへ変更します。

   ```toml
   site_url = "https://<org-or-user>.github.io/<repo>/"
   ```

3. GitHub Actions の実行権限を確認します。

   - Repository Settings
   - Actions
   - General
   - Workflow permissions
   - `Read and write permissions` を選択

## GitHub Pages 設定手順

このデモの workflow は、`gh-pages` ブランチ直下に本番サイトとPRプレビューを配置します。

GitHub Pages は次の設定にしてください。

- Source: `Deploy from a branch`
- Branch: `gh-pages`
- Folder: `/ (root)`

公開URLは次の形式です。

- 本番: `https://<org-or-user>.github.io/<repo>/`
- PRプレビュー: `https://<org-or-user>.github.io/<repo>/pr-123/`

## main push時の本番デプロイ

`.github/workflows/deploy-production.yml` が実行されます。

処理内容:

- `main` をチェックアウト
- Zensical をインストール
- `zensical build --clean` で `site/` にビルド
- `peaceiris/actions-gh-pages` で `gh-pages` ブランチへデプロイ
- `keep_files: true` で既存のPRプレビューを残す

## PRプレビューの確認方法

Pull Request を作成または更新すると、`.github/workflows/pr-preview.yml` が実行されます。

処理内容:

- PRのソースをチェックアウト
- Zensical をインストール
- `zensical build --clean` で `site/` にビルド
- `destination_dir: pr-${{ github.event.pull_request.number }}` で `gh-pages/pr-<PR番号>/` にデプロイ
- PRにプレビューURLをコメント

コメントされるURL:

```text
https://<org-or-user>.github.io/<repo>/pr-<PR番号>/
```

## PR close時のプレビュー削除

Pull Request が close されると、同じ `.github/workflows/pr-preview.yml` の `cleanup-preview` ジョブが動きます。

`gh-pages` ブランチをチェックアウトし、`pr-<PR番号>/` を削除してコミットします。

## 404になったときの確認ポイント

- GitHub Pages の Source が `Deploy from a branch` になっているか。
- Branch が `gh-pages` になっているか。
- Folder が `/ (root)` になっているか。
- `gh-pages` ブランチに `index.html` が存在するか。
- PRプレビューの場合、`gh-pages` ブランチに `pr-<PR番号>/index.html` が存在するか。
- Actions の `publish_dir` が Zensical の `site_dir` と一致しているか。このデモでは `site_dir = "site"` と `publish_dir: ./site`。
- Actions の権限が `Read and write permissions` になっているか。
- 初回デプロイ直後の場合、GitHub Pages の反映に数分かかっていないか。
- リポジトリ名やユーザー名を含むURLが正しいか。

## ローカル確認

Zensical をインストールしてローカルで確認できます。

```bash
pip install zensical
zensical serve
```

ビルドだけ確認する場合:

```bash
zensical build --clean
```
