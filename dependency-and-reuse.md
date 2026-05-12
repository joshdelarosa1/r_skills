# Dependencies and Reuse

## R version

R ≥ 4.4. Any installation method that produces a working R is acceptable — pick whatever
fits the environment (system package manager, prebuilt installer, conda, container image,
hosted notebook, etc.). Pin the actual version in `renv.lock`.

## `renv` is mandatory

- `renv` is required for every project.
- `renv.lock` is the source of truth for the dependency graph and is committed to the
  repo.
- `renv` supports a shared global cache so multiple projects do not redownload the same
  package binaries; treat that as a capability of the tool, not as a fixed filesystem
  path.
- `renv/library/` is project-local installed state — it is **not** committed.

```r
renv::init()      # one-time, when starting a new project
renv::snapshot()  # after adding or upgrading a package
renv::restore()   # on a fresh checkout, to install the locked versions
```

## Project setup

Use `usethis` to scaffold project-level files (`.Rprofile`, `.lintr`, `tests/testthat/`,
package skeletons). Do not hand-roll boilerplate that `usethis` already covers.

## Choosing dependencies

- Prefer well-established CRAN or Bioconductor packages — especially ones already in
  `renv.lock` or considered standard for the domain.
- Do not add a new dependency just to save a few lines of code.
- When adding a new dependency, the response must explain:
  1. Why it is needed.
  2. What alternatives were considered.
  3. The impact on `renv.lock` (added packages and their transitive deps).

## When to write custom code

When no good package fits, write the code yourself, but keep it:

- Small.
- Modular — placed under `R/` and called from scripts.
- Covered by `testthat` tests.

See [testing.md](testing.md) for the test conventions.

## What is committed and what is not

- **Commit**: `renv.lock`, `renv/activate.R`, `renv/settings.json`, `.Rprofile`.
- **Do not commit**: `renv/library/`, `renv/python/`, downloaded data under `data/raw/`
  beyond a small fixture, `output/` artifacts.

(Git workflow itself — staging, signing, branching — is the future `r-git` skill's
territory and is intentionally not handled here.)
