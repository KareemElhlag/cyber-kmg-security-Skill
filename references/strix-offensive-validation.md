# Offensive Validation via Strix (External Tool, Not a Skill)

`$cyber-kmg-security` is defensive-first: planning, review, hardening, triage. Strix is a
separate, real thing — an autonomous AI pentesting **agent** that runs actual reconnaissance
and exploitation attempts against a live target and produces working proofs-of-concept. It is
never loaded as skill content into this session's context; it is invoked as an external process,
exactly like `k6` or `dotnet test` already are in this project's other skills.

## Source

- **Project**: Strix — https://github.com/usestrix/strix (mirrored at
  https://github.com/KareemElhlag/strix)
- **Commit reviewed**: `a0f8ea2cd379c4bf56e665d3db2ba8a4f7c0683c` (2026-10-18)
- **License**: Apache 2.0
- **Nature**: Python CLI (`strix-agent` on PyPI) that runs its own LLM-driven agent loop inside
  a Docker sandbox, with real reconnaissance/exploitation tools. It is not vendored into this
  repo — it stays an installed tool, invoked by command, same as any other external scanner.

## When to use it

Only when the task is explicitly **authorized active penetration testing** against a target the
user owns or has clear authority over — never as a default step in a routine review. Reach for
the defensive domain library (`../domains/`) and `$kmg-agent-skill`'s own quality gates first;
Strix is for the controlled-exploitation step after scope is agreed, not a replacement for SAST,
dependency scanning, or code review.

## Mandatory pre-flight (every run)

Follow this skill's own [Authorization Boundary](../SKILL.md) in full before touching Strix. In
addition, specifically for Strix:

1. **Named target only.** Resolve the exact `--target` value(s) with the user first — a local
   directory (mounted **writable** into the sandbox; commit or stash first), a URL, or an API
   spec. Never point it at a target the user has not named.
2. **Scope via `--instruction`.** Always pass an explicit instruction narrowing focus (e.g. "test
   only the Accounting posting-route resolver's tenant isolation; do not touch billing or
   payment flows") — Strix's default is a broad, deep scan of everything reachable from the
   target.
3. **Non-interactive + fail-on for CI-shaped runs**: `--non-interactive --fail-on <severity>` so
   a run has a deterministic exit code instead of needing a human to read a transcript live.
4. **Budget and turn caps are mandatory, not optional**: always pass `--max-budget <usd>` and
   rely on the `--max-turns` default (500/agent) unless the user asks for a tighter cap. An
   unbounded run against a multi-tenant ERP is a cost and blast-radius risk by itself.
5. **Local/staging target preferred.** A live production target requires the same explicit
   safe-plan + rollback + monitoring + user approval this skill already requires for any active
   production test — Strix does not know about this project's tenants, financial-period locks,
   or ledger integrity rules, so a human must scope it away from real customer data.

## Example invocation (illustrative shape, not a default to copy blindly)

```bash
export STRIX_LLM="<provider/model the user has approved>"
export LLM_API_KEY="<key>"
strix --target https://staging.nexusecore.example \
  --instruction "Authorized scope: Accounting posting-route resolver and tenant isolation only. \
  Do not touch /billing or any payment-provider webhook. Time window: <start>-<end>." \
  --scan-mode standard \
  --non-interactive --fail-on high \
  --max-budget 25
```

Replace every placeholder with the user's actual, confirmed values — never invent a host, a
model name, or a budget number here.

## Handling output

- Results land in `strix_runs/<run-name>/` on the host running the CLI. Treat that directory like
  `security-temp/<run-id>/` in this skill's own evidence rules: it is raw evidence, must be
  `.gitignore`d, and never committed as-is.
- Summarize into this skill's own reporting format (see `reporting.md`): finding id, target,
  severity, exploitability, business impact (tenant/ledger exposure especially), and remediation
  — not a pasted raw transcript.
- A Strix finding is evidence to triage, not an automatic verdict: verify impact against this
  project's own code/data before labeling anything critical, same discipline as any other
  scanner result in this skill.

## What this integration deliberately does not do

- It does not give Strix standing authorization to run — every invocation still needs the
  explicit scope step above, every time.
- It does not merge Strix's own prompts/instructions into this skill's context; the agent runs in
  its own process and its own sandbox.
- It does not replace the defensive domain library — Strix validates exploitability, it does not
  replace hardening review, dependency scanning, or secret scanning.
