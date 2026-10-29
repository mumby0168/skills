---
name: pr-intent
description: Find the intent and history behind a PR, covering its linked tickets (GitHub Issues, Linear, Jira), their project and sibling tickets, and git history of the area it touches. Use when reviewing a PR, asking why a change exists, or asking which tickets relate to it.
---

Return a short **intent snippet** for one PR. Intent comes only from the PR text and ticket text; code shows what changed, never why.

Inputs: PR number, repo path, depth (`near`, `area` or `wide`; default `area`).

## Steps

1. **Collect references** from the PR title, body, branch name, commit messages, and `gh pr view <n> --json closingIssuesReferences`.

   | Tracker | Patterns | Fetch with |
   |---|---|---|
   | GitHub Issues | `#123`, `owner/repo#123`, `Fixes #123`, issue URLs | `gh issue view` |
   | Linear | `ABC-123`, `linear.app/.../issue/ABC-123`, branch `abc-123-title` | Linear MCP |
   | Jira | `KEY-123`, `*.atlassian.net/browse/KEY-123` | Atlassian MCP |

   A bare `ABC-123` fits Linear and Jira. A URL decides; otherwise the tracker the repo already links to; otherwise try both. A reference whose tracker tool is unavailable goes under Gaps.

2. **Fetch and widen by depth.**

   | Depth | Tickets | History |
   |---|---|---|
   | `near` | Linked tickets | Last 10 commits on the touched files |
   | `area` | Plus parent project and its open tickets | 3 months on the touched folders |
   | `wide` | Plus closed tickets in the project | 12 months, plus reverts and fix commits |

   History: `git log --oneline` on the touched paths. Keep only what bears on this PR: earlier attempts, reverts, bugs in the same code, a ticket that this PR finishes or conflicts with.

Done when every reference is fetched or listed as a gap, and history for the depth is read.

## Snippet

40 lines at most. The intent is the first thing the reviewer reads, so make it **scannable**: a little context first, then short items. Write in ASD-STE100 Simplified Technical English: short sentences, one idea each, active voice, the same word for the same thing. Use the terms in the repo's `GLOSSARY.md` (follow `GLOSSARY-MAP.md` when there is one). Each item is one line; say what a criterion means in plain words and keep its ticket number as a trailing tag.

```
## Intent
<one sentence: what this PR makes true, and for whom. "No stated intent" when none.>
- Source: <ticket id>: "<the shortest quote that carries the goal>"
- Rule: <the one invariant the change must keep, when the ticket states one>
- Scope: <where this PR sits in a stack or project, and what it leaves for later>

Acceptance (<"none stated" when none>):
- <criterion in plain words> (<story or AC id>)

Deviations from the ticket (omit when none):
- <what differs, needing sign-off>

## Related tickets
- <ID> <title> (<status>): <why it matters to this PR>

## Area history
- <sha or PR#> <subject>: <why it matters>

## Gaps
- <unfetched reference, missing ticket, or ambiguity>
```
