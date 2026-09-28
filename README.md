# Personal site (Quarto + GitHub Pages)

Free portfolio and notes site. GitHub builds and publishes it automatically on every push, so you do not need to install anything locally.

## One time setup

1. On GitHub, create a new PUBLIC repository named exactly:

       laurasalop03.github.io

   Do not add a README or .gitignore in the GitHub form (this folder already has them).

2. From this folder, push it up:

       git init
       git add .
       git commit -m "Initial site"
       git branch -M main
       git remote add origin https://github.com/laurasalop03/laurasalop03.github.io.git
       git push -u origin main

3. In the repo on GitHub: Settings, then Pages. Under "Build and deployment", set Source to "Deploy from a branch", branch `gh-pages`, folder `/ (root)`. Save.

   (The first push runs the Action, which creates the `gh-pages` branch. If it is not there yet, wait for the Action in the Actions tab to finish, then set this.)

4. Your site goes live at:

       https://laurasalop03.github.io

   (allow a few minutes the first time)

## Add a new note later

Add one file under `blog/posts/`, for example `blog/posts/my-backtest.qmd`, with a header like:

    ---
    title: "My backtest write up"
    date: "2026-10-05"
    categories: [backtest, thesis]
    ---

Then:

    git add . && git commit -m "New note" && git push

The Action rebuilds and republishes automatically. You can also add or edit files directly in the GitHub web UI and it publishes the same way.

## Things to personalize

- Email and LinkedIn are already filled in.
- `index.qmd`: add a profile photo (put `profile.jpg` in this folder and add `image: profile.jpg` under the `about:` block).
- `projects.qmd`: tune the wording.

## Preview locally (optional)

Only if you want to see it before pushing: install the Quarto CLI from quarto.org, then run `quarto preview` in this folder.
