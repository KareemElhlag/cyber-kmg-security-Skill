# Security Checklists Reference

Use this reference to select checks for authorized defensive review. Treat it as coverage guidance, not a promise that
every possible vulnerability exists in one list.

## Web And API Coverage

Map findings to OWASP Top 10 and ASVS where possible:

- Broken access control: object-level authorization, tenant filters, role/permission checks, forced browsing, mass actions.
- Cryptographic failures: weak algorithms, plaintext secrets, missing TLS assumptions, unsafe key storage, poor rotation.
- Injection: SQL, NoSQL, LDAP, command, template, expression, header, log, and report/query injection.
- Insecure design: missing abuse-case controls, weak approval flows, business logic bypass, replayable workflows.
- Security misconfiguration: verbose errors, debug endpoints, open admin panels, risky CORS, missing security headers.
- Vulnerable/outdated components: direct and transitive dependency risks, abandoned packages, vulnerable base images.
- Identification/authentication failures: password reset, MFA, session expiry, refresh tokens, lockout, token revocation.
- Software/data integrity failures: CI/CD trust, unsigned packages, unsafe deserialization, unverified updates.
- Logging/monitoring failures: missing audit events, sensitive logs, no alerting for high-risk actions.
- SSRF: URL fetchers, webhooks, importers, report renderers, image/document processors, metadata service access.

## NexusCore ERP Coverage

- Tenant id cannot be supplied by clients as authority.
- Every query that returns business data is tenant-scoped and permission-scoped.
- Branch and warehouse access is enforced server-side.
- Ledger, COGS, stock, and journal writes are append-only or audit-safe according to domain rules.
- Approvals cannot be replayed, skipped, downgraded, or applied to stale document versions.
- Money calculations use value objects and defined rounding/currency policy.
- File import/export enforces type, size, parsing safety, row limits, permission, and audit trail.
- Reports avoid hidden data leakage through aggregates, filters, exported columns, and cache keys.
- Background jobs carry tenant/user/service identity deliberately and are idempotent.
- Audit logs capture actor, tenant, branch/warehouse, object id, action, result, and correlation id.

## Mobile And Flutter Coverage

Use OWASP MASVS/MASTG for mobile-specific review:

- No secrets or production credentials in app bundles.
- TLS and certificate validation are not weakened in release builds.
- Secure storage is used for tokens and sensitive local data.
- Screenshots, logs, crash reports, analytics, and caches do not leak sensitive ERP data.
- Local offline data has encryption, expiry, sync conflict policy, and remote revocation behavior.
- Deep links validate authentication, tenant, object existence, and permission after app restore.
- API errors do not reveal secrets, stack traces, SQL, internal ids, or authorization hints.
- Release builds disable debug flags, test endpoints, permissive network config, and verbose logging.

## Docker And Runtime Coverage

Use OWASP Docker guidance and CIS Docker Benchmark concepts:

- Docker host and engine are patched.
- Docker daemon socket is not exposed to containers or unauthenticated TCP.
- Containers do not run privileged unless there is an approved, documented reason.
- Images use minimal base images, pinned versions, and non-root users where practical.
- Secrets are not baked into images, environment dumps, logs, or compose files committed to source.
- Published ports bind only where intended; local dev ports should prefer localhost binding.
- Containers have health checks, resource limits, read-only filesystems where feasible, and restricted capabilities.
- Compose networks separate databases, internal services, public APIs, and admin tools.
- Image scans and dependency scans are recorded with versions and remediation decisions.

## CI/CD And Supply Chain Coverage

- Protected branches, required reviews, and status checks exist for production paths.
- Pipeline secrets are scoped, rotated, masked, and not exposed to pull requests from untrusted contexts.
- Build scripts do not curl remote code into privileged shells without pinning and verification.
- Artifacts are traceable to commits and environments.
- Deployment credentials cannot be used from arbitrary jobs.
- Dependency update bots are paired with tests and review, not automatic production deployment.
- Generated code and migration scripts are reviewed for trust and data impact.

## Safe Tool Categories

- SAST: language/framework analyzer, Semgrep-style rules, CodeQL-style queries where configured.
- Dependency scanning: package audit, NuGet/npm/pnpm/pub audit equivalents, SBOM review.
- Secret scanning: committed key/token/password patterns with manual verification and rotation guidance.
- Container scanning: image vulnerabilities, Dockerfile and compose misconfiguration, Docker socket exposure.
- DAST: OWASP ZAP-style scans against authorized local/staging URLs with rate limits and authenticated context.
- Manual tests: authorization matrix, workflow abuse cases, upload/export/report tests, tenant isolation probes.

## References

- OWASP Top 10: https://owasp.org/Top10/
- OWASP Cheat Sheet Series: https://cheatsheetseries.owasp.org/
- OWASP MASVS/MASTG: https://mas.owasp.org/
- CIS Docker Benchmark: https://www.cisecurity.org/benchmark/docker
- OWASP Docker Security Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html
