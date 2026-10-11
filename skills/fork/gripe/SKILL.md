---
name: gripe
description: Turn a code complaint into an agreed fix and a tested guardrail.
disable-model-invocation: true
---

Handle one **gripe** per session. Keep turns short and focused on the current decision. Agree the code change before discussing enforcement.

## Flow

1. **Understand and challenge.** Read the reported code and nearby conventions. Accept a snippet, file, diff, or description. Identify the objection, desired outcome, and a legitimate counterexample to the proposed rule. Push back on blanket restrictions; agree what should change and what should remain allowed.

2. **Choose the change.** Present three concrete treatments using the format below. Recommend one and wait for the user to choose. Keep enforcement for the next turn.

3. **Prove a guardrail.** Inspect existing checks and their local/CI wiring first, including checks that exist but do not run. Prefer fast, deterministic enforcement in this order:
   - Existing compiler, formatter, linter, analyzer, or architecture test.
   - A small custom check in the existing toolchain, such as an ESLint rule, .NET analyzer, or Semgrep rule.
   - Written guidance when reliable detection fails, needs disproportionate machinery, or the user requests it. Use existing coding standards; otherwise scoped `AGENTS.md`. Keep `AGENTS.md` to pointers where standards exist.

   Use one subagent to test the strongest candidate in a scratch directory outside the project. If it fails, have that same subagent investigate up to three alternatives. Require bad examples to fail, the agreed replacement and legitimate counterexamples to pass, and relevant boundary and syntax variations to behave correctly. Report observed results and runtime, not confidence alone. Retain useful fixtures for the chosen check.

4. **Choose enforcement and scope.** Present the recommendation, evidence, limitations, and any new dependency or tooling addition. Wait for the user's choice before adding it to the repo. If candidates remain unreliable, show the failing examples and suggest narrower enforcement or guidance for the user to choose.

   Default to the affected service, package, or domain. Scan that scope and report existing violations and cleanup size. If substantial unrelated changes are needed, offer a separate cleanup PR with a subagent in a worktree; start only if accepted. Agree when enforcement becomes blocking. Keep the current change focused, with no silently failing gate or broad exclusions.

5. **Fix and verify.** Apply the agreed correction and guardrail. Preserve behavior unless a behavior change was agreed. Run relevant existing tests. For automated enforcement, verify the real project command fails on a temporary violation and passes after correction; also verify included and excluded paths. Remove temporary probes. Report the fix, evidence, and remaining limits briefly.

## Options format

Use concrete names and one sentence per tradeoff. Show the current code once, then a fenced code block for each option using the project's language and real names. Include enough surrounding logic and any extracted helper to compare behavior; avoid ellipses in the changed logic.

Example:

Current:

```ts
return article.isApproved ? (article.hasAssets ? "ready" : "blocked") : "draft";
```

**1. Guard clauses (recommended)**

```ts
if (!article.isApproved) {
  return "draft";
}

if (!article.hasAssets) {
  return "blocked";
}

return "ready";
```

Pro: Makes the decision order visible.
Con: Adds several return points.

**2. Named predicate**

```ts
function canPublish(article: Article): boolean {
  return article.isApproved && article.hasAssets;
}

if (canPublish(article)) {
  return "ready";
}

return article.isApproved ? "blocked" : "draft";
```

Pro: Names the eligibility rule.
Con: Splits the status decision between the helper and its caller.

**3. Local expansion**

```ts
if (article.isApproved) {
  if (article.hasAssets) {
    return "ready";
  } else {
    return "blocked";
  }
} else {
  return "draft";
}
```

Pro: Keeps the decision tree together.
Con: Retains the nesting.

Which treatment fits? We will choose a guardrail after this decision.
