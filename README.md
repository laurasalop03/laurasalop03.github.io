# laurasalop03.github.io

Source for my personal site: **https://laurasalop03.github.io**

A portfolio and a set of notes documenting my move into quantitative finance from a computer science and machine learning background. Projects, plus short technical write ups from my thesis and quant learning.

## Built with

- [Quarto](https://quarto.org) for the site and for rendering notebooks, code and LaTeX math.
- GitHub Pages for hosting.
- GitHub Actions to build and deploy automatically on every push, so no local tooling is required.

## Structure

```
index.qmd            Landing page and short bio
projects.qmd         Selected projects, ordered by relevance
blog/
  index.qmd          Notes listing (auto-generated)
  posts/*.qmd        Individual notes
_quarto.yml          Site configuration (title, navigation, theme)
styles.css           Small style overrides
.github/workflows/   Build and deploy pipeline
```

## Adding a note

Create a file under `blog/posts/`, for example `blog/posts/my-topic.qmd`:

```yaml
---
title: "My topic"
date: "2026-10-05"
categories: [backtest, thesis]
---
```

Write below the header in Markdown. Code blocks and LaTeX math (`$...$`) render directly. Commit and push:

```bash
git add . && git commit -m "New note" && git push
```

The site rebuilds and redeploys automatically in about a minute.

## Editing locally

Only if you want to preview before pushing: install the [Quarto CLI](https://quarto.org/docs/get-started/) and run `quarto preview` in this folder. Otherwise, editing files directly in the GitHub web UI works too, since the build runs in the cloud.
