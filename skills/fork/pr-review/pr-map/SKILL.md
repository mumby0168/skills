---
name: pr-map
description: Map a PR's shape as a changed-file tree grouped by folder, with entry points and a numbered stop plan ordered by meaning and risk. Use when reviewing a PR, asking where to start, or wanting an overview of what changed.
---

Return a **map snippet** for one PR: the tree a reviewer scans first, and the stops a guided review walks.

Inputs: PR number, repo path, diff ref.

## Steps

1. **List files** with status and size: `git diff --name-status` and `--numstat` on the diff ref.
2. **Fold noise** into one `skip:` line per folder: lockfiles, generated code, snapshots, vendored code, bulk fixtures or notes.
3. **Find entry points**: the endpoint, handler, consumer, command, UI route or skill file where the change starts running.
4. **Plan stops.** One stop is one concept: one call chain, one contract, or one test group. Aim for 3 to 9. Order:
   - The core change that carries the intent first.
   - Risky stops early: contracts, data, shared code.
   - Tests beside the code they prove.
   - Wiring and config last.

Done when every changed file sits in a stop or a `skip:` line.

## Snippet

60 lines at most. The tree uses `A`/`M`/`D`/`R` markers and the stop number; leave a space after the number for `pr-lenses` flags.

```
## Tree
# PR <n> · <files> files · +<add> -<del>
src/billing/
├── M InvoiceBuilder.cs            [1] entry: builds lines
├── A PausedMinutes.cs             [1]
└── tests/
    └── M InvoiceBuilderTests.cs   [2] tests for [1]
src/contracts/
└── M VisitCompleted.cs            [3] stored event
docs/                              skip: 4 files

## Stops
1. <title>: <files with hunk anchor text>. <one line on why this is a stop>
```
