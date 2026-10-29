---
name: review-comments
description: Draft PR review comments with severity labels (blocking, should, question, nit, praise), written for the author in plain simplified English. Use when turning review findings or notes into comments for a PR author.
---

Draft comments the author can act on without a follow-up question. Inputs: candidate findings with `path:line`, and the mode.

## Severity

| Label | Means | Drafted in |
|---|---|---|
| `blocking` | Must change before merge: wrong behaviour, data loss, security, broken contract | every mode |
| `should` | A real improvement; the author picks the timing | every mode |
| `question` | The diff does not answer it | `balanced`, `perfectionist` |
| `nit` | Taste or polish, safe to skip | `perfectionist` |
| `praise` | Something worth keeping | `perfectionist` |

## Writing

Write each comment for a reader who has not seen this review: a little context, then the point. Use ASD-STE100 Simplified Technical English: short sentences, one idea each, active voice, the same word for the same thing. Use the terms in the repo's `GLOSSARY.md` when there is one.

Each comment:

```
path:line · <label>
<What the code does now, in one sentence.>
<The problem, or the question, in one or two sentences.>
<The suggested change, as a code snippet when that is shorter.>
```

Then an **Open questions** list for the author: things the review could not settle.

Done when every candidate is a comment or is dropped by the mode, and each comment names its line, label and suggested change.

Post only when the user asks, and only the comments they approve, as one review: `gh api repos/{owner}/{repo}/pulls/{n}/reviews` with a `comments` array.
