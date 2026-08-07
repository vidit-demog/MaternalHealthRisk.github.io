<div align="center">

# Maternal Health Risk

**An exploratory dashboard on maternal health risk, built entirely in R Markdown.**

[Live site](#) · [Report](#) · [Risk Explorer](#)

</div>

---

## Overview

This repository is a static R Markdown website (`rmarkdown::render_site`) built on the
Kaggle [Maternal Health Risk Data Set](https://www.kaggle.com/datasets/csafrit2/maternal-health-risk-data) —
1,014 pregnancies monitored across hospitals, community clinics, and maternal health cares
via IoT-based risk sensors, each labeled with a clinician-assigned risk level (low / mid / high)
from six vitals: age, systolic and diastolic blood pressure, blood glucose, body temperature,
and heart rate.

The site has three pages: a landing page, a full exploratory analysis, and an interactive tool
that lets a visitor enter their own vitals and see how they compare against the cohort. It's
designed to knit and deploy the way a course or personal data-science project typically does —
`.Rmd` → `rmarkdown::render_site()` → static HTML → GitHub Pages — with no backend server.

## Pages

| Page | File | Contents |
|---|---|---|
| **Home** | `index.Rmd` | Dataset overview, variable definitions, cohort summary stats |
| **Report** | `report.Rmd` | Distributions by risk level, correlation structure, a data-quality note, and a baseline multinomial logistic classifier |
| **Risk Explorer** | `explore.Rmd` | Interactive k-NN comparison tool — enter your own vitals and see which risk band they resemble most |

All analysis code in `report.Rmd` is folded by default (`code_folding: hide`); click **Code**
above any table or figure to expand the exact R that produced it.

## Project structure

```
.
├── _site.yml              # site-wide config: navbar, theme, output options
├── index.Rmd               # → index.html
├── report.Rmd               # → report.html
├── explore.Rmd               # → explore.html
├── assets/
│   ├── custom.css          # theme overrides on top of the flatly bootswatch theme
│   └── header.html         # <head> includes (webfont)
└── data/
    └── Maternal Health Risk Data Set.csv
```

## Getting started

### Prerequisites

- [R](https://www.r-project.org/) (≥ 4.1) and [RStudio](https://posit.co/download/rstudio-desktop/)
- The following packages:

  ```r
  install.packages(c(
    "tidyverse", "plotly", "nnet", "broom",
    "jsonlite", "janitor", "scales"
  ))
  ```

### Data path

Every `.Rmd` loads the dataset with:

```r
read_csv("data/Maternal Health Risk Data Set.csv")
```

This assumes the CSV lives in the `data/` folder shipped with this repo. If you relocate it,
that's the one line to update — in `index.Rmd`, `report.Rmd`, and `explore.Rmd`. Nothing else
in the project depends on file location.

### Build

Open the project folder in RStudio (it will recognize `_site.yml` as a website project) and
either:

- click **Build → Build Website** in the Build pane, or
- run `rmarkdown::render_site()` from the console

Both knit all three `.Rmd` files and write `index.html`, `report.html`, and `explore.html`
into the project root. Open `index.html` in a browser to preview locally.

## Deploying to GitHub Pages

1. Create a repository — `<your-username>.github.io` if this should be your root personal
   site, or any other name for a project page.
2. Commit and push the whole project, including the rendered `.html` files, `_site.yml`,
   `assets/`, and `data/` — GitHub Pages serves the built HTML, not the `.Rmd` source, so the
   `.html` files need to be in the repo.
3. In **Settings → Pages**, set the source to the branch you pushed to (root directory).
4. Update the GitHub icon link under `navbar: right:` in `_site.yml` to point at this repo.

Re-run **Build Website** and push again after every edit to an `.Rmd` or `_site.yml` — GitHub
Pages only reflects what's already been knit.

## How the risk explorer works

`explore.Rmd` computes cohort means and standard deviations in R and embeds the cohort (plus
those summary stats) as JSON directly into the page via a knitr chunk. A small vanilla-JS
widget then standardizes whatever a visitor enters, finds the 15 nearest cohort records by
Euclidean distance, and has them vote on a risk level — the same standardization approach used
in `report.Rmd`'s correlation and modeling sections. Everything runs client-side; nothing
entered in the form is sent anywhere, which is also what makes it work on GitHub Pages without
a live R session.

## Tech

R Markdown · `rmarkdown::render_site()` · tidyverse · plotly · nnet · vanilla JS · Chart.js ·
GitHub Pages

## Data source & disclaimer

Data: [Maternal Health Risk Data Set, Kaggle](https://www.kaggle.com/datasets/csafrit2/maternal-health-risk-data),
collected via IoT-based risk monitoring across hospitals, community clinics, and maternal
health cares.

This project is exploratory and educational. The risk explorer describes which historical
records a set of inputs most resembles — it is not a diagnostic tool and was not validated for
clinical use.
