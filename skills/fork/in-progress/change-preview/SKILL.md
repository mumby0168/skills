---
name: change-preview
description: Preview a proposed code change before implementation from session context, a ticket, or a Markdown spec. Use when the user wants to see the likely file layout, behavior, merge risk, and gaps in the requirements.
---

# Change preview

Produce a read-only, evidence-backed preview of the change the request implies. The preview is a proposal for discussion, not an implementation plan with guaranteed file names.

## Gather the request

Use the explicit ticket or Markdown path when one is supplied; read its full body and relevant comments or linked decisions. Otherwise use the current session. Treat session decisions as context for an explicit source, and call out any conflict between them. Name the source and the requirements you used.

Inspect the relevant repository before predicting files. Trace the existing entry point, ownership, callers or consumers, tests, and any schema, API, or deployment boundary the request crosses. Read the project's glossary and ADRs where they apply. If the repository is unavailable, identify the missing codebase and limit the result to a clearly marked requirements-only sketch.

The inspection is complete when every predicted change area has a reason grounded in either a requirement or an observed code path. Show uncertainty where the repository does not settle an exact path.

## Present the preview

Use the project's domain language. Keep prose short and put each visual beside the point it explains.

### Intended change

State the user-visible behavior and the current behavior it replaces or extends. Separate settled requirements from assumptions.

### Projected change tree

Make a shallow, folder-first tree the main visual. Show the smallest set of directories and files needed to communicate ownership and scope. Mark entries `A` add, `M` modify, `D` delete, or `R` move; give each a short responsibility or reason. Mark uncertain paths as likely locations, or show the directory without inventing a file name. Include a deletion or move only when the old path and the reason to remove it are known.

For example, the shape is:

```text
src/
├── orders/
│   ├── M service.ts       # change the order rule
│   └── A policy.ts        # own the new rule
└── legacy/
    └── D order-rule.ts    # remove after callers move
```

This is a projected tree. Do not present it as a Git diff or claim files have changed.

### Behavior and evidence

Add one supporting visual only when the tree leaves an important behavior unclear: a call tree for control flow, a component tree for UI ownership, a `diff` sketch for a local change, or Mermaid for interactions across boundaries. Use the smallest view that answers the current question.

Name the behavior that would prove the change works and the existing test seam that could verify it. Distinguish planned evidence from tests or observations already run.

### Merge danger

Use the same terms as the `pr` skill: **Door** is one-way when merging creates effects that are costly or impossible to reverse; otherwise it is two-way. State what makes the call true, including data changes, public contracts, external consumers, and rollout or rollback requirements. Give a concrete **Blast Radius**: the users, systems, and paths that could be affected, with the most important failure mode. Assess expected merge impact from the requirements and codebase; label unverified dependencies as such.

### Spec gaps

Give the three most likely ways an implementer could get this change wrong, ranked by consequence and likelihood. For each, show the triggering ambiguity or overlooked contract, what would fail, the codebase evidence, and a question or testable acceptance criterion that would close the gap. If fewer than three grounded risks exist, report fewer; avoid filler.

### Slicing signal

Say whether the proposed change looks small enough for one coherent implementation or has boundaries that may need staged delivery. Name those boundaries without turning them into tickets. Keep projected file paths in this preview; carry durable behavior and decisions into `to-spec` or `to-tickets` instead. After a tracer-bullet ticket exists, `slice-preview` can plan safe implementation stages within that ticket.

Finish when the reader can see the likely change shape, the reason for each area, its reversibility, and the decisions needed before implementation. Do not edit code, publish tickets, or claim the preview is an actual PR.
