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

### Automated drift check

`~/.claude/scripts/check_ntq_skills_drift.py` reads the pinned commit from each `ntq-*/SKILL.md` `source:` field, queries upstream HEAD via `git ls-remote` (read-only, no clone), and flags when upstream has moved away from the pin. A systemd user timer (`ntq-skills-drift.timer`, daily 07:55) runs it and writes `~/.cache/ntq-skills/upstream-drift.json`; `morning-brief.sh` surfaces any drift in the 08:00 digest. The check only reads skills and never re-vendors — refreshing the pin stays the manual step above.

Run on demand:

- `python3 ~/.claude/scripts/check_ntq_skills_drift.py` — human summary (exit `0` in sync, `10` drift, `1` error)
- `python3 ~/.claude/scripts/check_ntq_skills_drift.py --json` — full result

After a refresh, bump the `source:` field in each refreshed skill (the check keys off it) and update the base commit line above.
