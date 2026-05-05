---
title: Pull Request Preview
description: Preview documentation for pull request workflows.
---

# Pull Request Preview

このリポジトリでは、GitHub Pull Request ごとにプレビューサイトを自動生成します。

## プレビューの仕組み

1. Pull Request を作成、更新、または再オープンすると、`.github/workflows/pr-preview.yml` が実行されます。
2. PR のブランチをチェックアウトし、`zensical build --clean` で `site/` をビルドします。
3. `peaceiris/actions-gh-pages` が `gh-pages` ブランチの `pr-<PR番号>/` へ成果物をデプロイします。
4. プレビュー URL が PR コメントとして投稿または更新されます。

## プレビュー URL の確認方法

PR のコメント欄に次のような URL が表示されます。

```text
https://<org-or-user>.github.io/<repo>/pr-<PR番号>/
```

このURLを開くと、PR の変更内容を反映したビルド済みサイトを確認できます。

## プレビューの更新タイミング

- PR に新しいコミットを追加すると、自動でプレビューが再ビルドされます。
- 既存のプレビューコメントは更新され、古いコメントが上書きされます。

## PR を閉じたときの挙動

PR が `closed` されると、同じワークフローの `cleanup-preview` ジョブが実行されます。

- `gh-pages` ブランチから `pr-<PR番号>/` ディレクトリが削除されます。
- 削除内容は `gh-pages` へコミットされます。

## 使い方のおすすめ

1. `docs/` 以下で変更を行います。
2. PR を作成して、コメントに表示されるプレビュー URL を確認します。
3. レビュー担当者と共有しながら内容を確認します。
4. 問題なければ `main` へマージします。

## トラブルシューティング

- プレビューコメントが表示されない場合は、Actions の `Deploy Pull Request preview` が成功しているか確認してください。
- `gh-pages/pr-<PR番号>/index.html` が存在するかを確認します。
- `gh-pages` ブランチの `keep_files: true` 設定により、既存のプレビューや本番サイトが保持されます。
- PR をクローズした後は、プレビューは自動で削除されるため、URL は 404 になります。
