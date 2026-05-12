# Standalone Script Documentation

These rules apply to every standalone R script under `scripts/` — anything intended to be
run end-to-end with `Rscript`. Functions under `R/` and notebook chunks have their own
conventions and are not in scope here.

## CHANGELOG block

The first non-empty lines of any standalone script, above all code:

```r
# CHANGELOG
# - 2026-02-18: Added required-column validation; ensured output dir exists.
# - 2026-01-30: Initial version.
```

Rules: newest entry first; date in `YYYY-MM-DD`; describe user-visible or behavior-changing
edits only; no WIP notes or TODOs in this block.

## File header block

Immediately after the CHANGELOG:

```r
# ============================================================================
# Script: <short name or filename>
# Purpose: <one to two sentences>
# Inputs: <data sources and expected columns / types>
# Outputs: <artifacts produced with paths>
# Assumptions:
#   - <bullet>
# How to run: <exact command>
# Notes: <optional — credentials, slow steps, known issues>
# ============================================================================
```

## Section blocks

Divide the script body with standardized section headers:

```r
# ---- <Section name> --------------------------------------------------------
```

Valid section names, in logical order:

1. `Setup`
2. `Config`
3. `Load data`
4. `Validate assumptions`
5. `Transform`
6. `Model`
7. `Visualize`
8. `Output`
9. `Sanity checks`
10. `Helpers`

One blank line before each section header. Sections appear in script-flow order.

## Section intro comments

Immediately after each section header:

```r
# What this section does:
#   - ...
# Why it matters:
#   - ...
# Assumptions:       (if applicable)
#   - ...
# Outputs:           (if applicable)
#   - ...
```

`What this section does:` is always required. The other fields may be omitted when not
applicable.

## Inline comments

Add inline comments **only** for non-obvious steps:

- Business-logic rules and domain filters
- Data quirks (e.g., `NA` revenue means cancelled orders)
- Date / time / timezone handling
- Regex or complex reshaping
- Performance or non-standard patterns
- Validation and error handling
- Workarounds for known issues

Place the comment above the line(s) it explains. Explain *why* first, then *what* if
needed. Do not comment obvious operations or restate what the code already says.

Good:

```r
# treat NA revenue as cancelled orders and remove before totals
sales <- sales |> dplyr::filter(!is.na(revenue))
```

Bad:

```r
# increment x by 1
x <- x + 1
```

## Validation and fail-fast

If the script reads external data, include a `Validate assumptions` section that:

- Checks required columns exist
- Asserts type expectations when relevant
- Creates the output directory before writing
- Fails with clear error messages

```r
required_cols <- c("order_id", "placed_at", "revenue")
missing_cols  <- setdiff(required_cols, names(orders))
if (length(missing_cols) > 0) {
  stop("Missing required columns: ", paste(missing_cols, collapse = ", "))
}

dir.create(here::here("output"), recursive = TRUE, showWarnings = FALSE)
```

## Output and logging

Put all file writes in the `Output` section. After writing, emit a short status line:

```r
message("Wrote ", nrow(result), " rows to: ", out_path)
```

## Formatting rules (script-specific)

- Line width ≤ 100 characters (also enforced project-wide; see
  [linting-and-formatting.md](linting-and-formatting.md)).
- `suppressPackageStartupMessages()` for package loads.
- One blank line between sections; never multiple consecutive blank lines.
- No trailing whitespace.
- Consistent `# ----` section separators throughout the file.

## Acceptance criteria

A standalone script passes when:

1. CHANGELOG block exists at the top, comment-only.
2. All required header fields are present.
3. Every section has its intro-comment block.
4. Non-obvious steps have explanatory comments; obvious steps do not.
5. Outputs and validations are clearly documented.
