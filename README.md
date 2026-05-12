# r-conventions skill

A self-contained Claude Agent Skill that establishes R conventions for data wrangling,
visualization, and statistics. Covers style and naming, dependencies (`renv`), data I/O
and performance, visualization (`ggplot2` / Quarto), error handling, testing
(`testthat`), linting, standalone-script documentation, and the default work loop.

Excludes git workflow and Shiny apps — those will be packaged as separate skills
(`r-git`, `r-shiny`).

## Layout

```text
r-conventions-skill/
├── SKILL.md                                    # the skill body (loaded on activation)
├── README.md                                   # this file
└── references/
    ├── data-io-and-performance.md
    ├── dependency-and-reuse.md
    ├── error-handling-and-logging.md
    ├── linting-and-formatting.md
    ├── script-documentation.md
    ├── style-and-naming.md
    ├── testing.md
    ├── visualization.md
    └── workflow-and-communication.md
```

The skill follows the [Agent Skills](https://agentskills.io) open standard. Frontmatter
uses only the standard `name` and `description` fields, so the skill is portable across
any client that implements the spec.

## Install

Copy this folder into your Claude skills directory so the installed path is
`<skills-dir>/r-conventions/SKILL.md`. The installed directory must be named
`r-conventions` (matching the skill's frontmatter `name`). After installing, restart
Claude Code (or reload skills) so the new directory is discovered.

## How it loads

- The skill `description` is always in Claude's context, so Claude can decide when to
  apply the skill.
- `SKILL.md` (~95 lines) loads when the skill is invoked.
- The `references/*.md` files load on demand — only when their topic is in play.

This keeps the always-on context cost small while the full rule set remains available
when needed.
