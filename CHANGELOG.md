# Changelog — Cyber KMG Security

## 2026-10-19 — Merge defensive domain library + offensive-validation entry point

**Request**: combine two external GitHub projects into this skill so it becomes a single
entry point for both defensive playbooks and authorized active exploitation, without turning
it into an untrusted-code liability.

**What was pulled and reviewed (read-only clone first, decision made only after reading)**:

1. `github.com/KareemElhlag/claude-security-skills` (fork of `GoldenWing-360/claude-security-skills`,
   commit `9e08941695d25d2b48abd36bcb93bc8531fc8c66`, MIT license) — 38 plain-Markdown,
   defensive-only skill files, no executable code, explicitly "no exploitation, no offensive
   tradecraft" per its own README.
2. `github.com/KareemElhlag/strix` (mirror of `usestrix/strix`, commit
   `a0f8ea2cd379c4bf56e665d3db2ba8a4f7c0683c`, Apache 2.0) — a real, separate Python CLI
   (`strix-agent`) that runs an autonomous LLM-driven pentesting agent inside a Docker sandbox,
   performing actual reconnaissance and exploitation with working proof-of-concept validation.

**Decision and why**: these two are architecturally different and were integrated differently,
not identically:

- The 38 skill files are inert reference content (plain Markdown + YAML frontmatter) — vendored
  byte-for-byte into `domains/` with full attribution in `domains/SOURCE.md` and the original
  `LICENSE.md` carried over. Zero execution risk; safe to load as skill content.
- Strix is a real agent that executes attacks — it was explicitly **not** vendored or merged as
  skill content. Instead it is documented as an external tool invocation in
  `references/strix-offensive-validation.md`, gated by this skill's pre-existing Authorization
  Boundary, with mandatory scoping (`--instruction`), budget (`--max-budget`), and non-interactive
  exit-code (`--fail-on`) requirements spelled out from Strix's own CLI reference docs (not
  guessed).

**Files added**:
- `domains/<38 folders>/SKILL.md` — vendored defensive playbooks.
- `domains/SOURCE.md` — provenance, commit SHA, update procedure.
- `domains/LICENSE.md` — upstream MIT license text.
- `references/strix-offensive-validation.md` — Strix invocation, scoping, evidence handling.
- `CHANGELOG.md` — this file.

**Files modified**:
- `SKILL.md` — new "Defensive Domain Library" and "Offensive Validation (Strix)" sections, plus
  workflow steps 4 and 8 pointing at them.
- `README.md` — new section describing the merge and listing the new reference/domain files.

**Not done / explicitly deferred**:
- Strix was not installed or run against any NexusCore target in this change — this is
  documentation and wiring only. The first real run still needs the user's explicit named
  target, scope instruction, and budget per the new reference doc.
- No attempt was made to keep `domains/` in sync automatically with upstream — updates are a
  deliberate, reviewed re-pull per `domains/SOURCE.md`'s own procedure, never automatic.
