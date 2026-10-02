# Site Roadmap, Ideas and Reference

A living, private notes file for the portfolio site. It is named with a leading
underscore so Quarto ignores it: it shows in the GitHub repo but never publishes as a
page. Edit it freely.

Live site: https://laurasalop03.github.io

---

## 1. How the site works (quick reference)

- Built with **Quarto** (`.qmd` files), hosted free on **GitHub Pages**.
- A **GitHub Action** builds and deploys on every push to `main`. The build runs in the
  cloud, so no local tooling is required to publish.
- Pages source (repo Settings, Pages) is set to **GitHub Actions**.
- Everyday loop: `git pull` → edit → `git add . && git commit -m "..." && git push`.
  The site rebuilds in about a minute.
- Working across computers: the repo is the source of truth. Pull before editing, push
  after. Each machine authenticates separately (its own SSH key or token).

### Repo structure
- `index.qmd` : home page and bio, featured card
- `about.qmd` : about page
- `projects.qmd` : projects list, each links to a detail page
- `projects/*.qmd` : one page per project
- `blog/index.qmd` : notes listing
- `blog/posts/*.qmd` : individual notes
- `blog/posts/_template.qmd` : note skeleton (Question, Method, Result, Limitations)
- `_quarto.yml` : site config (title, navbar, theme, search)
- `.github/workflows/publish.yml` : build and deploy pipeline

### Adding a note
Copy `blog/posts/_template.qmd` to `blog/posts/your-topic.qmd`, fill the header
(title, date, categories), write the four sections, set `draft: false`, then push.
Categories are freeform tags; keep them lowercase and consistent. Suggested set:
`thesis`, `backtesting`, `risk`, `time-series`, `machine-learning`, `optimization`,
`options`, `market-data`, `meta`.

---

## 2. Note ideas, expanded

Each note should keep the four-part shape (Question, Method, Result, Limitations),
which is the structure quant readers respect. Below each idea: the core question, the
method to use, the dataset, the likely result, and the honest limitation to call out.

### Risk

**1. Where normal-VaR breaks**
- Question: does the normal-distribution assumption underestimate tail risk?
- Method: compute 1-day Value at Risk three ways (parametric/normal, historical,
  Monte Carlo), then backtest exceedances with the Kupiec proportion-of-failures test.
- Data: a few years of daily returns for one liquid index or stock (yfinance).
- Likely result: the normal method is breached more often than its confidence level
  allows, especially in crisis windows; historical and Monte Carlo do better.
- Limitation: single asset, no volatility clustering modelled yet (sets up note 6).

**2. VaR vs Expected Shortfall**
- Question: what does VaR miss, and why is Expected Shortfall (CVaR) preferred?
- Method: compute both at 95% and 99%; show ES as the mean loss beyond VaR; explain
  coherence (subadditivity) with a small two-asset example.
- Likely result: ES captures tail severity VaR ignores; a diversification example where
  VaR is not subadditive but ES is.
- Limitation: estimation of ES is noisier in the tail; needs more data.

### Backtesting honesty

**3. An honest backtest**
- Question: does the SMA-crossover edge survive real-world frictions?
- Method: take the dashboard strategy and add transaction costs (bps per trade),
  slippage, and fix any look-ahead bias; compare net vs gross equity curves.
- Likely result: the apparent edge shrinks or disappears after costs.
- Limitation: one strategy, one asset; costs are assumed, not broker-exact.

**4. Is a backtest significant?**
- Question: is a good Sharpe real or luck from trying many strategies?
- Method: compute Sharpe and Sortino; introduce the deflated Sharpe ratio and the
  multiple-testing problem; simulate how max-Sharpe-over-N-random-strategies inflates.
- Likely result: with enough trials, an impressive Sharpe appears from noise alone.
- Limitation: assumes a return model for the simulation; real strategies are correlated.

### Market behaviour

**5. Do returns have memory?**
- Question: can you predict tomorrow's return from the recent past?
- Method: autocorrelation function, Ljung-Box test on returns; connect to weak-form
  efficiency.
- Likely result: little to no linear autocorrelation in returns (close to a random walk).
- Limitation: linear tests only; non-linear dependence may remain (lead to note 6).

**6. Volatility clustering and GARCH**
- Question: is volatility predictable even when returns are not?
- Method: show autocorrelation of squared/absolute returns; fit GARCH(1,1); forecast
  conditional volatility; compare to a rolling standard deviation.
- Likely result: strong persistence in volatility; GARCH reacts faster than rolling std.
- Limitation: GARCH(1,1) is basic; no leverage effect (EGARCH/GJR would extend it).

### Portfolio and derivatives

**7. Markowitz efficient frontier**
- Question: how do risk and return trade off across portfolios?
- Method: mean-variance optimisation on a handful of assets; plot the frontier; find the
  minimum-variance and maximum-Sharpe (tangency) portfolios.
- Likely result: a clean frontier; the tangency portfolio and the capital market line.
- Limitation: in-sample covariance is unstable out-of-sample (mention shrinkage).

**8. Black-Scholes from scratch**
- Question: how is a European option priced, and where does the model fail?
- Method: state assumptions, implement the formula, plot the Greeks (delta, gamma, vega,
  theta, rho); discuss the volatility smile as evidence of broken assumptions.
- Likely result: correct prices and intuitive Greek shapes.
- Limitation: constant volatility and no jumps; real markets show a smile/skew.

**9. Monte Carlo option pricing**
- Question: can simulation price an option, and how fast does it converge?
- Method: simulate geometric Brownian motion paths, average the discounted payoff, study
  error vs number of paths; add a variance-reduction trick (antithetic variates).
- Likely result: convergence to Black-Scholes for a European call; error shrinks like
  1/sqrt(N); variance reduction tightens it.
- Limitation: slow for high precision; path-dependent options need care.

### Strategy

**10. Pairs trading and cointegration**
- Question: can a mean-reverting spread between two related assets be traded?
- Method: test cointegration (Engle-Granger), build the spread and its z-score, define
  entry/exit thresholds, backtest with costs from note 3.
- Likely result: a plausible mean-reverting signal; honest after-cost performance.
- Limitation: cointegration can break (regime change); needs rolling re-estimation.

### A few more, if those run out
- **Sharpe ratio intuition**: what a Sharpe of 1 vs 2 vs 3 actually means, annualisation
  pitfalls, and why it rewards consistency.
- **Drawdown and recovery**: maximum drawdown, time-to-recover, Calmar ratio on a strategy.
- **Correlation is not stationary**: show rolling correlation between two assets spiking
  in crises (the "correlations go to 1" effect) and why it breaks diversification.
- **Kelly criterion**: optimal bet sizing, and why practitioners use fractional Kelly.
- **A simple factor**: sort stocks by one signal (momentum or value), form long-short
  deciles, inspect the spread. A gentle intro to cross-sectional thinking.
- **Bootstrapping returns**: use resampling to put confidence intervals on a backtest's
  Sharpe, instead of trusting a single number.

### Suggested order
Risk (1, 2) → backtest honesty (3, 4) → market behaviour (5, 6) → portfolio and
derivatives (7, 8, 9) → a combined strategy (10). Early notes extend the dashboard;
later ones lean on the maths degree.

---

## 3. How to improve the site in the future

### Content (highest impact for the quant goal)
- Publish 1-2 real notes before any LinkedIn launch. The notes are the differentiator.
- Add a **thesis page or note**: the adaptive algorithmic trading system (time series,
  volatility modelling, regime-change and concept-drift detection). This is a strong,
  unique story. A page now (even "in progress") plus a note when results exist.
- Add **screenshots or a short GIF** at the top of each project page for visual punch.
- Add result **figures** inside project pages, not just text.

### Features
- **Interactive charts in notes** (Plotly): a live equity curve or a returns distribution
  reads far better than static text. Needs a small build change (install Python + libs in
  the GitHub Action so it can execute chart code at build time). Add it with the first
  note that benefits.
- **Comments** (giscus): free, backed by GitHub Discussions. Enable Discussions, install
  the giscus app, add the `comments:` block to `_quarto.yml`.
- **Visitor analytics** (GoatCounter): free, privacy-friendly, no cookie banner. Register
  a name, paste the one-line script into the site config.
- **A downloadable CV (PDF)**, linked from Home and About.
- **A featured-note slot** on the home page (the stub is already there) pointing at the
  best note once one exists.
- **Render the dashboard's `investigation.ipynb` notebook** as a page, with almost no
  rewriting (Quarto renders notebooks natively).

### Polish
- A custom domain (not free, roughly the cost of a domain per year) if you want
  `lauras alop.com` instead of the github.io URL. Set it in repo Settings, Pages.
- Open Graph preview (a title, description and image when the link is shared on
  LinkedIn). Quarto supports it via `_quarto.yml` under `website:` `open-graph:` and
  `twitter-card:`. Worth doing right before the LinkedIn launch so the shared card looks
  professional.
- A short, specific "what to try" line on the dashboard page (for example: enter a ticker
  such as AAPL and run the backtest) so a visitor engages with the live demo immediately.
- Keep categories consistent so the Notes filter stays useful.

---

## 4. Launch checklist (for when the notes are ready)

- [ ] At least one, ideally two, polished notes published.
- [ ] Thesis mentioned somewhere visible (page or note).
- [ ] Each project page has a screenshot or figure.
- [ ] The live dashboard works end to end (ticker, backtest, risk, forecast).
- [ ] Open Graph / preview card configured so the shared link looks good.
- [ ] CV linked (if you choose to share it publicly).
- [ ] Proofread for typos; check every link.
- [ ] THEN post on LinkedIn, linking the site and calling out one concrete note.

---

## 5. Useful facts to remember

- Site repo (SSH): `git@github.com:laurasalop03/laurasalop03.github.io.git`
- Pages source must stay **GitHub Actions**.
- Commit identity uses the GitHub noreply email so the personal email stays out of public
  git history. On any new machine, set:
  `git config --global user.email "116661256+laurasalop03@users.noreply.github.com"`
- Live dashboard app: https://my-financial-dashboard.streamlit.app (deployed from the
  separate `financial-dashboard` repo, `master` branch, entry file `dashboard.py`, on
  Streamlit Community Cloud, free).
- Dashboard `requirements.txt` must list ONLY real deps: streamlit, pandas, numpy, plotly,
  scipy, yfinance, prophet. A system-wide `pip freeze` breaks the deploy.
- If a Streamlit deploy fails building prophet/scipy on a very new Python, set the app's
  Python version to 3.12 in the app settings and reboot.
- After pushing a dashboard fix, Streamlit auto-redeploys in a minute or two; if the error
  persists, hard-refresh the browser or reboot the app from Manage app.
