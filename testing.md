# Testing

## Where tests live

- `testthat` is required.
- Tests live in `tests/testthat/`.
- One test file per `R/` source file is the default convention
  (`R/foo.R` → `tests/testthat/test-foo.R`).

## What to test

- For every new or changed function, add or update at least one `testthat` test.
- Tests must be deterministic — no reliance on the network, system clock, random seeds
  without `set.seed()`, working directory, or environment variables.
- Tests must pass before merging or releasing.

## Running tests

After any R change, run the full suite and **show the output**:

```r
testthat::test_dir("tests/testthat")
```

Or from a shell:

```
Rscript -e 'testthat::test_dir("tests/testthat")'
```

If the repo has no documented test command and no `tests/testthat/` directory, do not
claim tests passed. Report verbatim: **"No test command specified in repo docs"** and
stop.

## YAML config validation

When R reads a YAML config (for example, a project-level config under `config/`), validate
it before claiming the change is done:

```r
yaml::read_yaml(here::here("config", "settings.yml"))
```

The call must return without error. YAML comment lines must start with `#` — do not leave
a line of comment text without the prefix, or the parser will treat it as content.

## R-side verification before "done"

Before reporting a task complete, run these and show their output:

1. `testthat::test_dir("tests/testthat")` — and show the output.
2. If R files were touched: `lintr::lint("path/to/changed/file.R")` — confirm no new
   warnings.
3. If a YAML config was touched: `yaml::read_yaml("path/to/file.yml")` — confirm no
   errors.

Git-side verification (which files changed, what is staged, commit status) belongs to the
future `r-git` skill and is intentionally not handled here.

## Required response summary

After any code change, see the **Required response summary** in
[workflow-and-communication.md](workflow-and-communication.md).
