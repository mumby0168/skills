---
name: slice-preview
description: Plan implementation stages for an existing tracer-bullet ticket. Use when the user wants to see whether that ticket should ship in one change or in smaller, safely mergeable steps.
---

# Slice preview

Keep the tracer-bullet ticket's outcome and acceptance criteria intact. Compare implementing it in one change with delivering it through smaller stages. A stage can be a PR, a tracked child ticket, or work within one PR; it is not automatically a new ticket.

## Read the parent ticket

Read the explicit ticket or Markdown ticket file, including relevant comments and linked decisions. If the user refers to the current ticket, resolve it from the session and read it. Ask for the ticket when no existing tracer-bullet ticket can be identified. Read an available change preview as supporting evidence, not as a substitute for the ticket.

Inspect the relevant repository contracts, callers, tests, migrations, and release path. Use the project's glossary and ADRs where they apply. The investigation is complete when you can state which parts must change together, which can land while existing behavior keeps working, and how the parent ticket's acceptance criteria will be met. If the codebase is unavailable, mark the comparison provisional.

## Compare implementation shapes

Start with one implementation change for the whole parent ticket as the baseline. Compare it with the smallest staged plan that addresses a real constraint: review size, independent ownership, migration order, compatibility, release control, or a risky one-way door. Prefer user-visible vertical increments when they can land cleanly. Use database, internal API, external API, or UI stages when each boundary has a useful proof and preserves a working system, even if the feature becomes visible only at the last stage.

For each proposed stage, identify:

- The behavior or contract it adds, changes, or retires.
- Its independent verification, including existing tests or the highest useful new test seam.
- Its predecessor and the reason for that dependency.
- Whether it can merge and deploy while previous behavior stays valid; otherwise identify it as an ordered step within one PR.
- Its user visibility and one-way or two-way door.
- The parent acceptance criteria it advances or completes.

Combine adjacent stages when their boundary cannot be verified or merged independently. Split a stage when it crosses a risky migration, public contract, or rollout boundary. For wide changes, consider **expand → migrate → contract**: add a compatible form, move consumers in safe batches, then remove the old form after evidence shows it is unused. Keep the final plan inside the parent ticket's scope; surface missing requirements as questions rather than silently broadening it.

## Present the decision

Show the one-change baseline and the staged alternative unless a concrete constraint rules one out. Recommend one and give the decisive reason. Use a dependency tree or compact Mermaid graph, followed by a table with one row per stage: deliverable, proof, dependency, merge condition, and parent acceptance criteria covered. Mark where the feature becomes usable. Show a cleanup stage only when the old path must later be removed. Account for every parent acceptance criterion and make any unsupported one visible.

Example shape, subject to codebase evidence:

```text
1. Add compatible data shape
   ↓
2. Add internal behavior
   ↓
3. Add external contract
   ↓
4. Connect UI and release       ← user-visible feature
   ↓
5. Retire old path, if needed
```

For a database/API/UI sequence, check compatibility at each edge. A database change is not automatically first; it belongs first only if later stages depend on it and it can land safely. If stages 1 and 2 need each other to pass or keep production working, combine them into one mergeable stage or keep them as ordered work within one PR.

End with the unresolved decision that most affects the split, if there is one. The parent ticket remains the source of truth. If the user wants tracked child tickets, explain that `/to-tickets` can create them later from the agreed stages and dependencies, with this ticket as the parent. Do not create issues or change tracker state in this skill.
