# Fork maintenance

This repository is a fork of [mattpocock/skills](https://github.com/mattpocock/skills). `origin` is `https://github.com/mumby0168/skills.git`; `upstream` is `https://github.com/mattpocock/skills.git`. Both default branches are `main`. At setup, local `main`, `origin/main`, and `upstream/main` pointed to commit `49dd158`. There were no fork-only commits or other working-tree changes before this maintenance setup.

## Changes this fork owns

| Path | Keep during upstream updates |
| --- | --- |
| `AGENTS.md` | A regular file and the single root agent-instructions file. It retains the upstream repository guidance plus this fork's maintenance and skill-edit policy. |
| `CLAUDE.md` | Removed. Do not restore the upstream root file or its `AGENTS.md` symlink. Preserve useful new upstream guidance by merging it into `AGENTS.md`. |
| `FORK.md` | This maintenance contract, including an accurate list of fork-owned changes. |
| `skills/fork/upstream-update/`, `skills/fork/README.md` | The fork's user-invoked upstream update skill and its index. Keep them local and separate from inherited skills. |
| `skills/fork/gripe/` | User-invoked code complaint workflow, tested guardrails, and Codex metadata. |
| `skills/fork/pr-review/` | Guided PR review: the user-invoked `guided-review` and its model-invoked scouts and presenters, with Codex metadata and the review log template. |
| `skills/fork/in-progress/` | Local `change-preview` and `slice-preview` skills, their Codex metadata, and their index. They are read-only and remain out of the plugin manifest until promoted. |
| `.claude-plugin/plugin.json`, `README.md` | Fork plugin identity, version, shipped skill registration, installation instructions, and fork skill links. Preserve unrelated upstream explanations. |
| `.claude-plugin/marketplace.json` | `mumby` marketplace and `mumby-skills` plugin identity, owned by Billy Mumby and sourced from this fork. |
| `package.json`, `package-lock.json`, `.changeset/config.json` | Fork package identity and GitHub changelog source. Package and plugin versions match; the first fork version is `1.3.1-mumby.1`. |
| `.agents/install-block.md` | Canonical Claude Code and Codex install commands for this fork. The root README copies them. |

Inherited skills remain unchanged by default. A user must explicitly instruct an edit to a named upstream skill. Add local behavior in `skills/fork/` and register it where needed. If a later explicit edit to an inherited skill creates a fork difference, add its path and reason to this table in the same change.

## Update from upstream

Run `/upstream-update` from this checkout, or follow these steps:

1. Check `git status --short --branch`, `git remote -v`, and the two branches with `git rev-list --left-right --count main...upstream/main`. Start with a clean working tree. If it is dirty, preserve that work and stop before merging.
2. Fetch with `git fetch upstream main` and compare again. Review incoming commits and files with `git log --oneline main..upstream/main` and `git diff --stat main...upstream/main`. Read this file and inspect every path in the table before integrating.
3. Create a new branch from `main`. Merge with `git merge --no-commit --no-ff upstream/main`. If the merge has conflicts, resolve each against the upstream change and this table. Keep `AGENTS.md` as a regular file, keep `CLAUDE.md` absent, and carry forward relevant upstream instruction changes. Do not resolve a conflict by blindly taking either side. Do not rewrite upstream skills to implement fork behavior without explicit user instruction.
4. Review the staged merge and any conflict resolutions. Check that all fork-owned paths above still satisfy their purpose and that unrelated upstream changes landed. Update this table when fork-owned changes are added or removed.
5. Run `git diff --check`, `npm run check-plugin-version`, and `claude plugin validate .` when the Claude CLI is available. Inspect the skill frontmatter and manifest entry. Run other checks required by the incoming change. Commit the merge only after validation; report any check that could not run.
6. Report the incoming commit range, conflicts and resolutions, validation, and remaining risks. Leave pushing and merging the update branch for a separate explicit request.

Keep history intact. Never force-push or silently overwrite working-tree changes during this process.

## Test the plugin

Run these commands yourself after reviewing the uncommitted changes. A local marketplace reads this checkout, including uncommitted files:

```bash
cd ~/repos/skills
claude update
claude --version
claude plugin validate .
claude plugin marketplace add "$PWD"
claude plugin install mumby-skills@mumby
codex plugin marketplace add "$PWD"
codex plugin add mumby-skills@mumby
```

Confirm `claude --version` reports v2.1.277 or later before relying on the root `AGENTS.md`. Start new sessions in each agent. Check `claude plugin details mumby-skills` and `codex plugin list --marketplace mumby` for the installed plugin. Check that `upstream-update` is discoverable. The local CLI at setup was 2.0.31 and did not accept `claude plugin validate . --strict`.

## Install from GitHub

After committing and pushing the fork, the same plugin can be installed from GitHub:

```bash
claude plugin marketplace add mumby0168/skills
claude plugin install mumby-skills@mumby
codex plugin marketplace add mumby0168/skills
codex plugin add mumby-skills@mumby
```

If the local marketplace is already registered, remove that local marketplace first, then add the GitHub source. The plugin name remains the same.

## Replace Matt's plugin

The current user-level installations are `mattpocock-skills@mattpocock` in both Claude Code and Codex. Once the fork loads and its inherited skills are discoverable in each agent, remove the old plugin:

```bash
claude plugin uninstall mattpocock-skills@mattpocock
codex plugin remove mattpocock-skills@mattpocock
```

The old `mattpocock` marketplace can then be removed from each agent if no other plugins use it:

```bash
claude plugin marketplace remove mattpocock
codex plugin marketplace remove mattpocock
```

Do not remove the Git `upstream` remote. It is part of the update process above.
