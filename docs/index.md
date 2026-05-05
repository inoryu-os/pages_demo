---
title: Home
description: Team knowledge site demo.
---

# Team Knowledge Demo

This repository is a minimal demo for publishing team knowledge documents with
Zensical and GitHub Pages.

## What This Demo Shows

- Markdown documents are stored on the `main` branch.
- Zensical builds Markdown into static HTML.
- The production site is deployed when `main` is updated.
- Pull Requests get a preview site at `/pr-<PR number>/`.
- Preview directories are removed when Pull Requests are closed.

## Recommended Workflow

1. Add or update Markdown files under `docs/`.
2. Open a Pull Request.
3. Review the generated preview URL.
4. Merge to `main` after review.
5. Confirm the production site is updated.

See [Operations](operations.md) for common maintenance notes.
See [Pull Request Preview](pull-request.md) for PR-specific preview workflow details.
