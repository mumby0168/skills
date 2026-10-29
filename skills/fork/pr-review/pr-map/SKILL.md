---
name: pr-map
description: Map a PR's shape as changed-file trees chunked by concern, with entry points and a numbered stop plan ordered by meaning and risk. Use when reviewing a PR, asking where to start, or wanting an overview of what changed.
---

Return a **map snippet** for one PR: the tree a reviewer scans first, and the stops a guided review walks.

Inputs: PR number, repo path, diff ref.

## Steps

1. **List files** with status and size: `git diff --name-status` and `--numstat` on the diff ref.
2. **Fold noise** into one `skip:` line per folder: lockfiles, generated code, snapshots, vendored code, bulk fixtures or notes.
3. **Find entry points**: the endpoint, handler, consumer, command, UI route or skill file where the change starts running.
4. **Suggest stops.** One stop is one concept: one call chain, one contract, or one test group. Aim for 3 to 9. They are suggestions the reviewer picks from; list them in this order:
   - The core change that carries the intent first.
   - Risky stops early: contracts, data, shared code.
   - Wiring and config last.
5. **Chunk the tree by stop.** Each stop gets its own small tree with every file that makes up that concern: source, its tests, and its contract or generated files together. A file that serves two stops sits in the first, tagged `also [k]`. Fold a run of similar files into one line (`Core/Consolidation/ (8 files)`). The `skip:` lines go last, after every chunk.

Done when every changed file sits in exactly one chunk or a `skip:` line.

## Snippet

60 lines at most. One chunk per stop, in stop order. A chunk opens with its stop number and title, then a tree whose top lines are folders from the repo root, with any prefix every changed file shares dropped and named once in the header. Files carry `A`/`M`/`D`/`R` markers; leave a space after the marker column for `pr-lenses` flags.

```
## Tree
# PR <n> · <files> files · +<add> -<del> · under services/
[1] Invoice lines (risk: high)
billing/src/Invoices/
├── M InvoiceBuilder.cs            entry: builds lines
└── A PausedMinutes.cs
billing/tests/Invoices.Tests/
└── M InvoiceBuilderTests.cs

[2] Stored event (risk: high)
billing/src/Contracts/
└── M VisitCompleted.cs            stored event field
gateway/Billing.Gateway.json       generated contract

skip: docs/ 4 files · 12 test files with 1-line ctor fixups

## Stops
1. <title> (risk: high | med | low): <hunk anchor text>. <one line on why this is a stop>
```
