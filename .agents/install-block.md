# Fork installation commands

The root [README.md](../README.md) copies these commands. The marketplace name is `mumby`; the plugin name is `mumby-skills`. Keep both names stable after the first install.

## Claude Code

After the fork is pushed:

```bash
claude plugin marketplace add mumby0168/skills
claude plugin install mumby-skills@mumby
```

For a local test before pushing, replace `mumby0168/skills` with the absolute checkout path. Run `claude plugin validate .` first. The local CLI at setup was 2.0.31 and rejects `--strict`.

Claude Code 2.1.277 or later reads the fork's root `AGENTS.md` directly. Run `claude update` before relying on those repository instructions on an older version. Plugin installation and `AGENTS.md` loading are separate.

## Codex

After the fork is pushed:

```bash
codex plugin marketplace add mumby0168/skills
codex plugin add mumby-skills@mumby
```

For a local test before pushing, replace `mumby0168/skills` with the absolute checkout path. Restart Codex to discover the installed skills.

## Replacing the upstream plugin

Confirm `mumby-skills@mumby` and its skills load in each agent before running the upstream removal commands in [FORK.md](../FORK.md#replace-matts-plugin). Keep the upstream Git remote for future merges.
