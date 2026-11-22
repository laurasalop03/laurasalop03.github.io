# laurasalop03.github.io

Source for my personal site: **https://laurasalop03.github.io**

A portfolio and a set of notes documenting my move into quantitative finance from a computer science and machine learning background. Projects, plus short technical write ups from my thesis and learnings.

## Built with

- [Quarto](https://quarto.org) for the site and for rendering code and LaTeX math.
- GitHub Pages for hosting.
- GitHub Actions to build and deploy automatically on every push, so no local tooling is required.

The code and notebooks behind the notes live in a separate repository: [research-blog](https://github.com/laurasalop03/research-blog).

## Structure

```
index.qmd              Landing page: short bio, featured project, latest note
about.qmd              About me
projects.qmd           Selected projects, ordered by relevance
projects/*.qmd         One detail page per project
blog/
  index.qmd            Notes listing (auto-generated, with RSS feed)
  posts/*.qmd          Individual notes
  posts/_metadata.yml  Settings shared by all notes (comments)
  posts/_template.qmd  Skeleton for a new note (not published)
assets/                Profile photo, favicon and figures for the notes
latest-post.ejs        Template for the "Latest Article" card on the home page
_quarto.yml            Site configuration (title, navigation, theme, metadata)
theme.scss             Shared theme: fonts, accent colour, styling
theme-dark.scss        Dark mode colours
styles.css             Small style overrides
analytics.html         GoatCounter analytics snippet
.github/workflows/     Build and deploy pipeline
```

## Local preview

```bash
quarto preview
```
