# Cyber KMG Security

Authorized defensive security review, penetration-test planning, hardening, and evidence logging for KMG/NexusCore-style ERP systems.

## Install Locally

Copy or clone this folder into your Codex skills directory:

```powershell
C:\Users\hp\.codex\skills\cyber-kmg-security
```

Then invoke it in Codex with:

```text
$cyber-kmg-security
```

## Use Inside A Project

For passive review or planning:

```text
Use $cyber-kmg-security to create a security test plan for this API.
```

For NexusCore production-grade work, combine it with KMG:

```text
Use $kmg-agent-skill and $cyber-kmg-security to run an authorized defensive security review.
```

For Flutter/mobile security work, combine it with the Flutter skill:

```text
Use $flutter-architect-review and $cyber-kmg-security to review this Flutter app security posture.
```

## Helper Scripts

- `scripts/init-security-run.ps1`: creates `security-temp/<run-id>/` and adds it to `.gitignore` and `.dockerignore`.
- `scripts/security-inventory.ps1`: inventories security-relevant files.
- `scripts/run-safe-baseline.ps1`: performs a safe first-pass scan for potential secrets, Docker risk patterns, and dependency manifests.

## Safety Boundary

This skill is for systems you own or are explicitly authorized to test. It is designed for defensive assessment, evidence, triage, and hardening.

## Main Files

- `SKILL.md`: skill entry point.
- `references/methodology.md`: authorization, scoping, and safe execution.
- `references/checklists.md`: web/API/mobile/container/CI coverage.
- `references/reporting.md`: security finding and run-record format.
- `references/strix-offensive-validation.md`: how (and when) to invoke Strix, an external
  autonomous pentesting agent, for explicitly authorized active exploitation.
- `domains/`: 38 vendored, defensive-only domain playbooks (AI/LLM, web app, infra, distributed
  systems, compliance, incident response, mobile — see `domains/SOURCE.md` for exact provenance).

## Defensive Domain Library + Offensive Validation

This skill now merges two external sources into one entry point:

1. **38 defensive-only playbooks** (MIT, vendored unmodified) — `domains/`, picked by matching
   attack surface rather than re-derived from scratch each time.
2. **Strix** — a real, separate autonomous pentesting agent (Apache 2.0), invoked as an external
   process for active exploitation only when the user has explicitly authorized a named target
   and scope. It is never loaded as skill content; see `references/strix-offensive-validation.md`.

Both are reference/tooling additions only — the Authorization Boundary above still governs every
check, defensive or offensive, that this skill ever runs.
