---
name: r-conventions
description: Use when working in an R project — writing or modifying R scripts, .qmd or
  .Rmd reports, R data-wrangling pipelines (dplyr, tidyr, readr, vroom, purrr),
  arrow/parquet datasets, httr2 HTTP calls, ggplot2 visualizations, statistical models
  with stats/broom, testthat tests, or YAML configs that R reads. Excludes git workflow
  and Shiny apps (separate skills).
---

# R Conventions

## Overview

The working contract for R analysis, reporting, and statistics. Reproducibility-first,
tidyverse-leaning, `snake_case`, namespaced calls, `here::here()` paths, `renv` for
dependencies, `testthat` for coverage. Aim for code that scales from analysis to reporting
to production-ish scripts, on any machine R itself runs on.

Out of scope: git workflow (use the future `r-git` skill) and Shiny apps (use the future
`r-shiny` skill).

## When to Use

Use this skill when an R-related task is in front of you, including:

- Reading, writing, joining, pivoting, or summarising tabular data in R
- Reading or writing CSV, TSV, parquet, or Arrow datasets from R
- Making HTTP requests or downloading data from R
- Authoring a `.qmd` (Quarto) or `.Rmd` report
- Building a `ggplot2` chart
- Fitting or summarising a statistical model
- Writing or modifying a standalone R script under `scripts/`
- Adding or changing an R function under `R/`
- Writing or modifying a `testthat` test under `tests/testthat/`
- Validating or editing a YAML config that R will read
- Setting up a new R project (`renv`, project layout, `.lintr`)

Do not use this skill for: git operations or commit workflow (future `r-git` skill);
Shiny app structure, `ui.R` / `server.R` layout, or modules (future `r-shiny` skill);
README authoring; secrets handling.

## Quick Reference

Each topic link goes to the per-topic reference file. Read the linked file when the topic
is in play; this list is for orientation, not the full rule.

- [R version, dependencies, `renv`, `usethis`](references/dependency-and-reuse.md) —
  R ≥ 4.4 (any install); `renv.lock` is the source of truth.
- [Style and naming](references/style-and-naming.md) — `snake_case`; `here::here()` +
  `file.path()` for paths; namespaced calls (`pkg::fn()`); native pipe `|>` for new repos;
  `%||%` + `NA` handling; default project layout.
- [Data I/O and performance](references/data-io-and-performance.md) — `readr` /
  `vroom` with explicit `col_types`; `httr2` for HTTP; Arrow filter-before-collect;
  vectorized first then `purrr::map*()`; large-data heuristic; performance checklist.
- [Visualization and reporting](references/visualization.md) — Quarto (`.qmd`) first;
  `ggplot2` + `theme_minimal()` + title + axis labels.
- [Error handling and logging](references/error-handling-and-logging.md) — selective
  `tryCatch()` by default; strict + `cli` logging in production code.
- [Testing](references/testing.md) — `testthat` in `tests/testthat/`; deterministic;
  per-function coverage; YAML config validation; R-side verification before "done".
- [Linting and formatting](references/linting-and-formatting.md) — `lintr` advisory
  locally / enforced in CI; `styler` on changed files; line width ≤ 100.
- [Standalone script documentation](references/script-documentation.md) — CHANGELOG,
  file header, section blocks, intro comments, validation, acceptance criteria.
- [Workflow and communication](references/workflow-and-communication.md) — default
  work loop; required response summary; pragmatic-teacher tone.

## Common Mistakes

These are the recurring failure patterns. The per-topic references explain why.

- Path mistakes (`setwd()`, hard-coded paths, OS-specific separators):
  see [`style-and-naming.md`](references/style-and-naming.md).
- `library(tidyverse)` and bare function names in production:
  see [`style-and-naming.md`](references/style-and-naming.md).
- Reading a CSV without `col_types`:
  see [`data-io-and-performance.md`](references/data-io-and-performance.md).
- Calling `collect()` before `filter()`/`select()` on Arrow / Parquet:
  see [`data-io-and-performance.md`](references/data-io-and-performance.md).
- Per-row work (`rowwise()`, `apply(..., 1, ...)`) on large frames:
  see [`data-io-and-performance.md`](references/data-io-and-performance.md).
- New or changed function without a `testthat` test:
  see [`testing.md`](references/testing.md).
- Standalone script missing CHANGELOG / header / section structure:
  see [`script-documentation.md`](references/script-documentation.md).
- Reporting "done" without running tests and showing the output:
  see [`testing.md`](references/testing.md).
- Generic identifier names (`df`, `data`, `x`, `tmp`):
  see [`style-and-naming.md`](references/style-and-naming.md).
- Adding a new package dependency without rationale:
  see [`dependency-and-reuse.md`](references/dependency-and-reuse.md).

## Out of Scope

- Git workflow, commit messages, branch policy, signed commits → future `r-git` skill.
- Shiny apps: `ui.R` / `server.R` layout, modules, reactive structure → future `r-shiny`
  skill. (Performance and error-handling rules in this skill still apply to R code that
  happens to run inside Shiny.)
- README authoring conventions.
- Secrets, credentials, or filesystem permissions.
