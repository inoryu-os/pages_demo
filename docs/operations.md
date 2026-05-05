---
title: Operations
description: Operational notes for maintaining the knowledge site.
---

# Operations

## Editing Documents

Create Markdown files under `docs/`. Zensical builds the directory into static
HTML under `site/`.

## Preview URLs

Pull Request previews are deployed to directories named after the Pull Request
number.

For example, Pull Request `123` is published at:

```text
https://<org-or-user>.github.io/<repo>/pr-123/
```

## Production Updates

When changes are merged into `main`, the production site is rebuilt and
deployed.

```text
https://<org-or-user>.github.io/<repo>/
```

## Cleanup

When a Pull Request is closed, its preview directory is removed from the
deployment branch.
