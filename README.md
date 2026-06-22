# NTQ Skills

Canonical source-of-truth for personal `ntq:` skills used from both Claude Code and Codex.

## Convention

- Skill directories use `ntq-<slug>`.
- Skill frontmatter uses `name: ntq:<slug>`.
- Runtime directories must point here with symlinks instead of hand-copied files.
- Vendored skills keep a `source` field in `SKILL.md` and a verbatim MIT `NOTICE`.

## Runtime Links

- Claude Code: `~/.claude/skills/ntq-*`
- Codex: `~/.agents/skills/ntq-*`

## Drift Policy

The current upstream base is `mattpocock/skills@6eeb81b5fcfeeb5bd531dd47ab2f9f2bbea27461`.

Review upstream periodically, for example quarterly, and rebase or refresh from that pinned lineage only when the update is worth the maintenance cost. Keep NTQ customizations small, re-run symlink/link checks after each refresh, and commit every change here so runtime state remains reversible.
