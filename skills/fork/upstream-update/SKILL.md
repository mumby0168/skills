---
name: upstream-update
description: Update this fork from Matt Pocock's upstream repository while preserving fork-owned changes.
disable-model-invocation: true
---

# Update the fork

The user invoked this skill to update the fork. Read the root `FORK.md`; it is the maintenance contract. Work from the repository root.

1. Inspect `git status --short --branch`, `git remote -v`, current branch, and `FORK.md`. Confirm `origin` and `upstream` against the documented URLs. If the worktree is dirty, preserve it and stop before fetch or merge; report what blocks a clean update. Add the documented upstream remote if missing. Do not replace an unexpected remote silently.
2. Fetch the upstream default branch. Compare `main` and `upstream/main` with `git rev-list --left-right --count`, `git log`, and `git diff --stat`. If there is no incoming change, report that and stop. If local `main` has diverged, inspect the local commits before choosing the integration branch.
3. Create a fresh update branch from local `main`. Read each fork-owned path in `FORK.md` and the incoming changes to it. Merge `upstream/main` with `--no-commit --no-ff` so the result can be reviewed before commit.
4. Resolve every conflict deliberately. Preserve the purpose of each fork-owned change. Keep `AGENTS.md` a regular file and `CLAUDE.md` absent, folding in relevant new upstream guidance. Keep local skills in `skills/fork/`. Treat edits to inherited skills as user-directed only. For conflicts requiring an upstream skill edit without that instruction, stop and explain the choice needed. Never use blanket `ours` or `theirs` resolution.
5. Review the complete staged result and check every contract row. Update `FORK.md` in the same change when fork-owned paths or reasons change. Run the checks specified in `FORK.md` and any checks required by incoming changes. Fix failures caused by the merge; disclose unavailable checks.
6. Commit the validated merge on the update branch. Report upstream commit range, changed areas, conflicts, validation, and branch name. Leave the branch ready for review. Push or merge it only if the user separately requests that action.

Keep existing history and working-tree changes safe. Never force-push, reset away changes, or overwrite files silently.
