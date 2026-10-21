# laurasalop03.github.io

Source for my personal site: **https://laurasalop03.github.io**

A portfolio and a set of notes documenting my move into quantitative finance from a computer science and machine learning background. Projects, plus short technical write ups from my thesis and learnings.

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

## Editing locally

If you want to preview before pushing: install the [Quarto CLI](https://quarto.org/docs/get-started/) and run `quarto preview` in this folder.
