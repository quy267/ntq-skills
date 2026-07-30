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

## Skills

Vendored from `mattpocock/skills` (MIT), renamed to the `ntq:` namespace, with cross-references rewritten to `ntq:<slug>`:

- **Design / modeling:** `codebase-design`, `domain-modeling`, `improve-codebase-architecture`
- **Spec / planning:** `grilling`, `grill-with-docs`, `to-prd`, `to-issues`
- **Build ops:** `resolving-merge-conflicts`
- **Learning / writing:** `teach`, `handoff`, `edit-article`

`codebase-design`, `domain-modeling`, and `handoff` carry NTQ customizations (Java/Go/Spring analysis lens, evidence rules, Multi-AI Review harness hooks) on top of upstream — preserve them across refreshes.

Add or refresh a skill with the helper (rewrites frontmatter + NOTICE + cross-refs, writes to a dir you choose so refreshes can be staged and diffed before overwrite):

```bash
python3 ~/.claude/scripts/vendor_ntq_skill.py --ref <sha> \
  --src-path skills/<category>/<slug> --slug <slug> \
  --dest-root ~/ntq-skills --ntq-slugs <comma-list-of-ntq-slugs>
```

## Drift Policy

The current upstream base is `mattpocock/skills@16a2a5cd00b4416f673f4ff38c7971a04dd708e7`.

Review upstream periodically, for example quarterly, and rebase or refresh from that pinned lineage only when the update is worth the maintenance cost. Keep NTQ customizations small, re-run symlink/link checks after each refresh, and commit every change here so runtime state remains reversible.

### Automated drift check

`~/.claude/scripts/check_ntq_skills_drift.py` reads each skill's `source: <repo>@<pin>` and, for the files that skill actually vendored, compares the upstream **blob SHA** (content hash from the git tree) at the pin vs current HEAD. It classifies each skill `unchanged` / `changed` (a vendored file's upstream content moved) / `moved` (the upstream dir was renamed). Because it compares only vendored files' content — not the pinned sha — it ignores the ~100 unrelated commits/month and upstream-only files (e.g. `agents/*.yaml`), so it flags a skill only when that skill genuinely warrants review. A systemd user timer (`ntq-skills-drift.timer`, daily 07:55, `SuccessExitStatus=10`) runs it and writes `~/.cache/ntq-skills/upstream-drift.json`; `morning-brief.sh` surfaces the changed/moved skills in the 08:00 digest. Read-only (GitHub tree API + `git ls-remote`); never re-vendors — refreshing a skill stays the manual step above.

Run on demand:

- `python3 ~/.claude/scripts/check_ntq_skills_drift.py` — human summary (exit `0` all vendored skills current, `10` ≥1 changed/moved, `1` error)
- `python3 ~/.claude/scripts/check_ntq_skills_drift.py --json` — full result

After a refresh, bump the `source:` field in each refreshed skill (the check keys off it) and update the base commit line above.
