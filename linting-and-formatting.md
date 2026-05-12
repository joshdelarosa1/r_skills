# Linting and Formatting

## Tools

- **`lintr`**: advisory locally, enforced in CI. Configure at the project level via
  `.lintr` at the project root.
- **`styler`**: apply on changed files before commit. Do not bulk-restyle unrelated files
  in the same change.

## Cross-file formatting rules

These rules apply to every R file (`.R`, `.qmd`, `.Rmd`, tests):

- Line width ≤ 100 characters.
- No trailing whitespace.
- Use `suppressPackageStartupMessages()` when attaching packages whose startup messages
  add noise.
- One blank line between logical sections; never multiple consecutive blank lines.
- Use a consistent section separator throughout a file:
  `# ---- Section name --------------------------------------------------------`.
  See [script-documentation.md](script-documentation.md) for the standalone-script
  section convention that uses these separators.

## Quick commands

```r
lintr::lint("R/foo.R")
styler::style_file("R/foo.R")
```
