# Visualization and Reporting

## Reports

- New reports: Quarto (`.qmd`).
- Legacy `.Rmd` is allowed when the project already uses it.
- Render with `quarto render path/to/report.qmd`.

## Plots

- Use `ggplot2`.
- Apply `theme_minimal()` as the default theme.
- Every plot must have a title and axis labels that name the variable being plotted in
  human-readable units (no placeholder text such as `x` or `y`).

```r
ggplot2::ggplot(monthly_sales, ggplot2::aes(x = month, y = revenue, colour = region)) +
  ggplot2::geom_line() +
  ggplot2::labs(
    title = "Monthly revenue by region",
    x     = "Month",
    y     = "Revenue (USD)",
    colour = "Region"
  ) +
  ggplot2::theme_minimal()
```
