# Data I/O and Performance

## I/O defaults

- **HTTP**: use `httr2`. Compose requests, then `httr2::req_perform()`.
- **CSV / TSV**: use `readr::read_csv()` or `vroom::vroom()`. Specify column types when
  the schema is stable — silent type-coercion on the next refresh is a common source of
  data quality bugs.
- **Parquet / Arrow**: use the `arrow` package. Filter and select **before** `collect()`,
  so only the columns and rows you actually need are pulled into R memory.
- **Caching**: when a read is expensive and the source changes infrequently, cache the
  typed intermediate (e.g., as parquet) and document the cache invalidation rule in
  comments.

```r
# correct: filter and select on the Arrow dataset, then collect
sales <- arrow::open_dataset(here::here("data", "sales")) |>
  dplyr::filter(year == 2026) |>
  dplyr::select(region, customer_id, revenue) |>
  dplyr::collect()
```

```r
# correct: explicit col_types
orders <- readr::read_csv(
  here::here("data", "orders.csv"),
  col_types = readr::cols(
    order_id   = readr::col_integer(),
    placed_at  = readr::col_datetime(),
    revenue    = readr::col_double()
  )
)
```

## Iteration

- Vectorized operations first. Most tidyverse and base verbs are already vectorized.
- For non-vectorizable iteration, default to `purrr::map*()` for readability.
- `for`, `lapply`, or `vapply` are acceptable on hot paths, but justify the choice with a
  `bench::mark()` snippet that shows the speedup.
- Avoid per-row work: `dplyr::rowwise()` and `apply(df, 1, ...)` should be a last resort.

## Large-data heuristic

Pick the engine by size and join shape:

| Situation                                     | Engine                                    |
| --------------------------------------------- | ----------------------------------------- |
| < ~1–2 M rows, fits comfortably in RAM        | `dplyr` / `tidyr`                         |
| Larger, memory pressure, or repeated joins    | `data.table` with keyed joins             |
| Source on disk, columnar, partitioned         | `arrow` (filter / select before collect)  |

Joins: declare keys explicitly, and select only the columns you need before joining. Avoid
repeated `mutate()` chains on very large frames — each chain materializes a new copy.

## Performance checklist

Trigger this checklist whenever data is **> ~200 MB**, **> ~1 M rows**, or the work runs
**inside a loop or a Shiny reactive**:

1. State the expected data size and a target wall-clock time.
2. Confirm join and sort complexity. Avoid quadratic patterns.
3. Filter and select early; do not materialize wide intermediates.
4. Add a `bench::mark()` snippet around the hot section.
