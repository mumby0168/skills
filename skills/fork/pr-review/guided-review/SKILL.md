---
name: guided-review
description: Walk me through a PR: an HTML overview first, then terminal sparring stop by stop.
disable-model-invocation: true
---

Guide the reviewer through one PR. The reviewer builds the understanding and makes every call; you are the **guide**. Research runs in subagents so this conversation holds only their snippets and the sparring.

Arguments, all optional and in any order:

- PR: number or URL. Default: the PR for the current branch (`gh pr view`).
- Mode: `pragmatist`, `balanced` (default) or `perfectionist`. Pass it to every scout and to `review-comments`.
- Depth: `near`, `area` or `wide`. Default: auto, set in step 1.

## Flow

1. **Size.** Run `gh pr view <pr> --json number,title,body,author,url,headRefName,baseRefName,additions,deletions,changedFiles,commits`. Fetch the head without touching the working tree: `git fetch origin pull/<n>/head:pr-<n>`; the diff is `git diff origin/<base>...pr-<n>`. Set auto depth:
   - `near`: under 200 changed lines in one module, with no contract touched.
   - `wide`: over 1000 changed lines, or touches a contract (stored events, migrations, public API, message schemas) or shared code.
   - `area`: everything else.

   Done when PR facts, mode and depth are known.

2. **Scout.** Dispatch three subagents in parallel, one each for `pr-intent`, `pr-map` and `pr-lenses`. Each prompt tells the subagent to call the Skill tool with its scout's name and passes the PR number, repo path, diff ref, mode and depth. `pr-lenses` also gets the intent snippet when you run it after `pr-intent`; parallel is fine, and it then judges ticket fit from the PR body alone. Done when all three snippets are back.

3. **Overview.** Call the Skill tool with `review-log` to create the overview page from the three snippets. Print its full absolute path on its own line. In the terminal, say only: the why in one sentence, the stops and focus areas as **suggestions** (the riskiest marked), and "pick any, or name your own area". Done when the page exists and the reviewer has picked.

4. **Stops.** The reviewer drives, in any order, and may name an area outside the suggestions; treat it as a new stop. For each stop:
   - Read only that stop's hunks.
   - Show one real excerpt (25 lines at most) with short inline notes, then one or two lines of why.
   - For a behaviour change, add a **scenario walk**: one named example traced through the new code.
   - For a contract change, show a real payload as a `diff` block of JSON: `-` lines before, `+` lines after, values taken from tests, fixtures or seed data. Then name each producer, consumer and stored copy it reaches, with `CHECK` on anything unverified.
   - Spar: answer with `path:line` evidence; say "not verified" for anything you have not run.
   - Draw only when a picture carries what the excerpt cannot: components connect differently, a before/after topology, a state machine, a flow across services. Pass it to `review-log` with the stop.
   - **Checkpoint**: ask "In your words, what does this stop do?" If the answer misses something, name the gap in one line.
   - Keep each candidate comment the reviewer wants, with its `path:line`.
   - Call the Skill tool with `review-log` to append the stop.

   Done per stop when the explain-back and candidates are in the log.

   Reviewer commands: `next` (take the top suggestion), a stop number, `overview`, `deeper` (one subagent re-runs the relevant scout on this stop only), `spread` (re-run `pr-intent` one depth wider), `done`.

5. **Wrap up.** On `done`, or after the last stop, call the Skill tool with `review-comments` for the candidates and mode, then with `review-log` to finish the page with the comments, open questions and unvisited stops. Print the full absolute path again.

   Comments stay drafts. Post only after the reviewer confirms, in this conversation, the exact comments to post; then post those and nothing else. Without that confirmation, never post.

## Turn shape

Short turns: one stop, one excerpt, one question. Lead with code, explain in a sentence. The reviewer asks for more when they want it.
