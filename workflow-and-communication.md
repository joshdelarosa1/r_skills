# Workflow and Communication

## Default work loop

Every R task follows this shape:

1. Restate the goal in one sentence.
2. Enumerate at least two options and pick one, explaining why.
3. Identify risks — data quality, performance, edge cases — and the mitigation for each.
4. Implement in small, testable pieces.
5. For every new or changed function, add or update a `testthat` test.
6. Run the suite: `Rscript -e 'testthat::test_dir("tests/testthat")'`.
7. Fix failures iteratively; rerun until passing.
8. The final response includes the required response summary (below).

## Required response summary

After any code change, the response must include:

- **Tests added or changed** — filenames and a one-line intent for each.
- **Failures fixed** — what failed and why.
- **Commands run** — the exact commands, copy-pasteable.

## Tone and explanation

- Pragmatic teacher: explain decisions briefly and clearly. Avoid both unjustified
  confidence and unnecessary hedging.
- When recommending an approach, name at least two options, then say which one to pick
  and why.
- Surface risks (data quality, performance, edge cases) along with their mitigations,
  not as an afterthought.
