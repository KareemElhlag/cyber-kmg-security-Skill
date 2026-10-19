# Source and Attribution — Defensive Domain Library

The 38 `SKILL.md` files in this directory are vendored, unmodified, from an external
open-source project. They are reference material only: `$cyber-kmg-security`'s own
`SKILL.md` decides when and how to apply them; nothing here executes on its own.

## Origin

- **Project**: Claude Code Security Skills
- **Upstream repository**: https://github.com/GoldenWing-360/claude-security-skills
- **Pulled from fork**: https://github.com/KareemElhlag/claude-security-skills
- **Commit pulled**: `9e08941695d25d2b48abd36bcb93bc8531fc8c66` (2026-08-03)
- **License**: MIT (Copyright (c) 2026 GoldenWing-360) — see `LICENSE.md` in this directory
  for the exact license text carried over with the vendored files.
- **Nature**: 38 defensive-only Markdown playbooks, YAML frontmatter, no executable code.
  The upstream project states explicitly: "No exploitation, no offensive tradecraft."

## Why vendored instead of referenced live

Pulled as plain files (not a git submodule or live fetch) so that:
- This skill works fully offline / air-gapped, matching this project's existing skills.
- A future upstream change cannot silently alter instructions this skill relies on —
  updates are a deliberate, reviewed re-pull, never automatic.
- Every file's provenance is traceable to one exact commit, named above.

## Integrity

Each file was copied byte-for-byte from the pulled commit with no edits. If a domain file
here ever needs a NexusCore-specific addition, add it as a new section in `$cyber-kmg-security`'s
own `SKILL.md` or a new file under `../references/`, not by editing a vendored file in place —
that keeps the diff against upstream always re-verifiable.

## Updating

To refresh this set:
1. Clone the upstream (or the fork) at the commit/tag to pull.
2. Diff its `*/SKILL.md` files against this directory.
3. Replace changed files wholesale (never hand-merge), update the commit SHA above, and
   note the change in `$cyber-kmg-security`'s own changelog if one exists.
4. Re-run `$kmg-agent-skill`'s skill-doctor equivalent check if available, to confirm the
   skill set still loads cleanly.
