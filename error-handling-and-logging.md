# Error Handling and Logging

## Default: selective

Wrap a call in `tryCatch()` only when the failure is recoverable, or when the surrounding
code needs to attach context to the error before re-raising. Do not blanket-wrap every
call — a clean stack trace is more useful than a swallowed error.

## Production scripts: strict + structured logging

For standalone scripts (`scripts/*.R`) and any code that runs unattended (CI, scheduled
jobs, Shiny apps), be strict and log:

- Use `tryCatch()` at boundaries that genuinely need recovery or context.
- Use the `cli` package for user-facing status, warnings, and errors:
  `cli::cli_inform()`, `cli::cli_warn()`, `cli::cli_abort()`.
- Fail fast on invalid inputs — see the `Validate assumptions` pattern in the
  [script-documentation.md](script-documentation.md) reference.

```r
result <- tryCatch(
  load_and_transform(input_path),
  error = function(e) {
    cli::cli_abort(c(
      "Failed to process input.",
      "i" = "Path: {input_path}",
      "x" = conditionMessage(e)
    ))
  }
)
```
