---
name: pr-lenses
description: Light-touch PR check that flags what a reviewer should look at, through four lenses (tests that prove behaviour, fit to the ticket, fit with codebase patterns, and blast radius). Use when reviewing a PR or asking what to watch for; returns flags, not verdicts.
---

Return a **flags snippet** for one PR. A flag points the reviewer at something worth a look; the reviewer decides. Stay shallow: the diff plus one hop of callers or callees per flag.

Inputs: PR number, repo path, diff ref, mode, and the intent snippet when available.

## Mode

| Mode | Flags |
|---|---|
| `pragmatist` | Wrong behaviour, broken contracts, data loss, security, tests that prove nothing |
| `balanced` | Plus pattern misfits, missing edge cases, unclear names |
| `perfectionist` | Plus readability, structure and documented style. Read the repo's coding standards; if a `code-style` skill is available, call the Skill tool with it |

## Lenses

Run blast radius first; it sets how hard to look with the rest.

- **Blast radius.** Classify the widest reach: shared capability, then crosses a service boundary, then local. Name each contract touched: stored events, migrations, public API, message schemas, config keys, feature flags, existing data.
- **Tests prove behaviour.** For each behaviour change, find its test and ask: would this test still pass if the code did the wrong thing? Note behaviour changes with no test.
- **Fits the ticket.** Against the stated intent: what the diff adds beyond it, what it leaves out, and what should have changed but did not.
- **Fits patterns.** Look beside the change for an existing helper or convention it bypasses, and for a new abstraction with one use.

Done when every changed behaviour has passed through all four lenses.

## Snippet

15 flags at most, most important first:

```
## Blast radius
<local | crosses service boundary | shared capability>: <contracts touched>

## Worth a look
- path:line · <lens> · <what to look at> · <why it might matter>

## Not checked
- <what this pass did not do: nothing run, no build, callers beyond one hop, ...>
```
