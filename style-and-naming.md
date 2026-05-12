# Style and Naming

These rules apply to every R file in the project — `.R`, `.qmd`, `.Rmd`, and
`tests/testthat/`.

## Naming

- `snake_case` for variables, functions, and filenames.
- Avoid generic names: `df`, `data`, `x`, `tmp`. Names should describe the thing
  (`sales_by_region`, not `df`).

## Pipe operator

- New repos use the native pipe `|>`.
- Legacy repos may keep `%>%` (magrittr).
- One pipe per repo, not both. Document the choice in the project README or `.lintr`.

## Paths

Always use `here::here()` to address paths from the project root, and `file.path()` to
compose path segments. Never call `setwd()`. Never hard-code OS-specific separators or
home-rooted paths.

```r
# correct: portable across operating systems
csv_path <- here::here("data", "raw", "sales.csv")
out_path <- file.path(here::here("output"), "summary.parquet")
```

`here::here()` finds the project root by looking for marker files (`.Rproj`, `.git`,
etc.), so the same code resolves correctly whether the project is opened from the repo
root or from a subdirectory.

## Namespaces in scripts

In standalone scripts and production code, call functions by their package: `dplyr::filter()`,
`tidyr::pivot_longer()`, `readr::read_csv()`. Avoid `library(tidyverse)` and bare function
names in production. Interactive exploration in a `.qmd` chunk may attach packages, but
the script that runs in CI should not.

## `%||%` and `NA`

When using the null-coalescing operator `%||%`, also handle `NA` explicitly. `%||%` only
catches `NULL`, so `NA` slips through:

```r
value <- (raw_input %||% default_value)
if (is.na(value)) value <- default_value
```

## CLI argument parsing

Guard `commandArgs()` so a script behaves correctly both when run as `Rscript script.R ...`
and when sourced into an interactive session:

```r
if (!interactive()) {
  args <- commandArgs(trailingOnly = TRUE)
  # parse args here
}
```

## Default project layout

Rules elsewhere in this skill assume this layout. Use it for new projects.

```text
.
├── R/                  # functions and helpers (sourced from scripts and tests)
├── scripts/            # standalone analysis scripts (Rscript-runnable)
├── reports/            # Quarto / Rmd source
├── data/               # input data (gitignore if large)
├── output/             # generated artifacts
├── tests/testthat/     # testthat tests
├── renv.lock           # locked dependencies (committed)
├── renv/               # renv state (renv/library/ is gitignored)
├── .Rprofile           # project R startup (sources renv/activate.R)
├── .lintr              # lintr config
└── README.md
```
