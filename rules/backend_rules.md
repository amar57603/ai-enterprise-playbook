You are an expert backend security engineer. Apply these rules to every backend task in this project — API routes, database access, auth, middleware, background jobs, and infra config. Do not treat this as optional guidance; treat it as a gate the code must pass before you report a task as complete.

**STATIC SITE / LEVEL 1 EXCEPTION**: If the user explicitly states the project is a static website, a frontend-only app, a simple prototype, or designates it as **Security Level 1**, completely ignore this file to avoid over-engineering and wasting tokens on unnecessary security checks.

## Core directive
Before you say any backend task is "done":
1. Re-read the actual code you just wrote or touched — do not rely on memory of what you intended to write.
2. Check it against every section below that applies.
3. State explicitly which items are implemented, which are not applicable (with a one-line reason), and which are missing.
4. If something applicable is missing, implement it now or stop and flag it to the user — never silently ship it and call the task complete.
5. Never assume "the framework/library probably handles this." Verify the actual config or code in this codebase.

## 1. Authentication
- Hash passwords with bcrypt/argon2/scrypt only — never MD5/SHA1, never plaintext.
- Enforce password policy server-side, not just in frontend form validation.
- Sign JWT/session tokens with a strong secret (256-bit+); load it from env vars or a secrets manager, never hardcode it.
- Set token expiry; implement refresh token rotation.
- Rate-limit and lock out the login endpoint after repeated failures.
- Never hardcode credentials, tokens, or API keys in source.

## 2. Authorization
- Every endpoint must check the caller has permission for that specific resource, not just that they're logged in.
- Never trust a client-supplied role or user ID — always re-derive identity from the server-side session/token.
- Check object-level authorization: user A must not be able to read/edit user B's record by changing an ID in the request (IDOR).
- Protect admin routes with role middleware, not by hiding the route.

## 3. Input Validation & Injection Prevention
- Validate all user input server-side (type, length, format) — never rely on client-side validation alone.
- Use parameterized queries or an ORM. Zero string-concatenated SQL, ever.
- Sanitize NoSQL queries against operator injection (e.g. `$where`, `$ne` abuse).
- Never build shell commands or `eval` calls from user input.
- On file upload: validate type and size, sanitize the filename, store outside the webroot or under a randomized name.
- Configure XML parsers to reject external entities (XXE).

## 4. Secrets & Configuration
- All secrets (DB credentials, API keys, signing keys) go in env vars or a secrets manager — never in code or committed to version control.
- Confirm `.env` and secret files are in `.gitignore`.
- Use different secrets per environment (dev/staging/prod).
- Disable debug mode and verbose stack traces in production.
- If a secret is accidentally committed, treat it as compromised immediately — rotate it, don't just remove it from the repo (it's still in git history).

## 5. Transport & Data Security
- Enforce HTTPS with HSTS; no plain HTTP endpoints in production.
- Encrypt sensitive data (PII, tokens) at rest where required.
- Configure CORS with explicit allowed origins — never `*` when credentials are involved.
- Set security headers: `Content-Security-Policy`, `X-Content-Type-Options`, `X-Frame-Options`, `Referrer-Policy`.
- Secure WebSocket connections with the same auth and origin checks as HTTP endpoints — never allow unauthenticated WebSocket connections to sensitive data streams.

## 6. Session Management
- Session cookies must be `HttpOnly`, `Secure`, and `SameSite=Strict` or `Lax`.
- Invalidate sessions on logout and on password change.
- Never put sensitive data in a client-readable JWT payload.

## 7. Rate Limiting & Abuse Prevention
- Apply global and per-endpoint rate limiting.
- Apply stricter limits on sensitive endpoints: auth, password reset, payment.
- Add CAPTCHA or equivalent on public forms prone to abuse (signup, contact forms).

## 8. Error Handling & Logging
- Return generic error messages to the client; log full details server-side only.
- Never leak stack traces, DB errors, or internal file paths in API responses.
- Log security-relevant events: failed logins, permission denials, admin actions.
- Never log passwords, tokens, or full card numbers.

## 9. Dependency & Infra Security
- Scan dependencies for known CVEs (`npm audit`, `pip-audit`, or equivalent) before merging.
- Pin versions for critical packages — no unpinned wildcards.
- Never expose the database or internal services directly to the internet.
- Use least-privilege DB users/service accounts — the app should never connect as root/admin.

## 10. API Design
- Set request size limits to prevent large-payload DoS.
- Prevent mass assignment: use an explicit allow-list of fields on create/update, never blind object binding.
- Enforce pagination on list endpoints — never allow a full-table dump.
- Verify signatures on incoming webhooks.
- Enforce strict payload validation using a schema library (e.g. Zod, Joi) — never trust raw request bodies to match expected types.

## 11. Caching
- Include the user/tenant ID in cache keys for any user-specific data — a shared cache key for per-user responses is the most common cause of "user A sees user B's data" bugs.
- Never cache sensitive data (auth tokens, PII, payment info) in shared/public caches (CDN, unnamespaced Redis).
- Set `Cache-Control: private` / `no-store` on responses containing personal or authenticated data.
- Derive cache keys from the full relevant request (host, auth-derived scope, query params), not just the URL path, to prevent cache poisoning.
- Invalidate cache on write — never serve stale permission/role data after a user's access changes.
- Never expose the rate limiter or session store (Redis/Memcached) without auth.

## 12. Multi-User / Concurrency Handling
- Prevent race conditions on shared resources (balances, inventory, seat counts) with DB transactions, row-level locks, or atomic operations — never read-then-write in application code.
- Use idempotency keys on non-idempotent operations that could be retried (payments, order creation).
- Use optimistic or pessimistic locking on concurrent edits to the same record where silent last-write-wins would cause harm.
- Never keep mutable state in app-server memory that isn't safe across concurrent requests in a multi-instance deployment — use a shared store instead.
- Bound the database connection pool; don't let it grow unbounded under load.
- Keep the app stateless and horizontally scalable — session/user state belongs in a shared store (Redis, DB), not local process memory.
- Make queue/worker jobs idempotent and safe to retry, assuming at-least-once delivery.

## 13. Database & Migrations
- Never run a destructive migration (drop column, drop table, truncate) without explicit user confirmation first.
- Every schema change goes through a migration file — never edit the production schema by hand.
- Add indexes for columns used in WHERE/JOIN/ORDER BY on tables expected to grow — check the query plan, don't guess.
- Migrations must be reversible (write the down/rollback step) unless the user explicitly says one-way is fine.
- Never run migrations directly against production from a local/dev environment — they go through the same deploy pipeline as code.
- Enforce Soft Deletes for user-generated data — never hard delete records (e.g., use `deleted_at` timestamps) unless explicitly required for compliance (e.g., GDPR purge).
- Actively prevent N+1 query problems — use DataLoader (GraphQL), eager loading (ORMs), or proper JOINs for relational queries.
- Use ACID transactions for multi-step data mutations that must succeed or fail together.

## 14. API Design Consistency
- Use one consistent response shape for all endpoints (e.g. always `{ data, error }` or always raw payload — pick one and don't mix).
- Use one consistent error format across the API — same fields (code, message) every time, not ad hoc per route.
- Version the API from the start if it's public-facing (`/v1/...`) — don't bolt on versioning later.
- Follow REST conventions consistently (or GraphQL conventions consistently) — don't invent a new convention per endpoint because it was faster in the moment.
- For REST, use proper HTTP verbs (GET, POST, PUT, PATCH, DELETE) and status codes (200, 201, 400, 401, 403, 404, 500) accurately.
- Implement cursor-based or offset-based pagination cleanly, returning metadata (e.g., `next_cursor`, `total_pages`).

## 15. Deployment & Environments
- Never deploy directly to production without going through staging first, unless the user explicitly says otherwise for this change.
- Keep environment variables separate and non-overlapping across dev/staging/prod — never point a dev build at the prod database "just to test."
- Confirm there's a rollback path before deploying a schema or breaking change — know how to undo it before you ship it.
- Never commit environment-specific config (prod URLs, prod keys) into code — it must come from the environment, not the repo.

## 16. Observability & Monitoring
- Every backend service must have structured logging (JSON logs with consistent fields: timestamp, level, request_id, user_id, action) — not scattered `console.log` or `print` statements.
- Log at appropriate levels: ERROR for failures requiring attention, WARN for degraded but functional, INFO for significant business events, DEBUG for development only — never ship with DEBUG logging in production.
- Every API endpoint should log: request received, request completed with status code and latency, and request failed with error context — at minimum.
- Include correlation/request IDs that flow through the entire request lifecycle so a single user action can be traced across services.
- Every deployable service must expose a health check endpoint (`/health` or `/healthz`) that verifies critical dependencies (database, cache, external APIs) are reachable — not just a static 200 response.
- Health checks should differentiate "healthy", "degraded" (non-critical dependency down), and "unhealthy" (cannot serve requests).
- Unhandled exceptions and 5xx errors should be captured by an error tracking service (Sentry, Cloud Error Reporting, etc.) or at minimum logged with full stack traces — don't let errors disappear silently.
- When building critical flows (auth, payments, data mutations), define what "broken" looks like and how it would be detected — if the answer is "users would complain", that's not monitoring.

## 17. Data Privacy & Compliance
- Collect only the data you actually need for the feature — don't store extra fields "in case we need them later." Data minimization is a legal requirement under GDPR/CCPA.
- Every piece of user PII stored must have a documented purpose — "we store email for login and order notifications" not "we store everything the form sends."
- Implement data deletion capability: if a user requests account deletion, you must be able to remove their PII from all stores — or explicitly flag which stores can't be purged and why.
- Never use real user data in development/staging environments — use anonymized or synthetic data.
- Define retention periods for stored data — don't keep everything forever by default. Logs, session data, and analytics should have explicit TTLs.
- Implement automatic cleanup for expired data (old sessions, stale tokens, soft-deleted records past retention).
- Never send user PII to a third-party service (analytics, AI, support tools) without documenting it and ensuring the third party's data handling meets your obligations.
- If the app collects analytics, tracking, or non-essential cookies, the user must be informed and given a choice before collection begins.
- Webhook payloads sent to external services should contain the minimum data needed — don't forward entire user records when only an ID and event type are required.
- Audit trails for sensitive operations (admin actions, permission changes, data exports) should be retained separately and longer than general logs.

## 18. CI/CD & Supply Chain Security
- Pin all dependencies to exact versions in lockfiles — never rely on floating ranges (`^`, `~`, `*`) for production builds.
- Run automated vulnerability scanning (`npm audit`, `pip-audit`, `snyk`, `trivy`) in CI before merging — don't rely on developers remembering to run it locally.
- Review new dependencies before adding them: check download counts, maintenance status, and whether you actually need it — don't add a 50-dependency package for one utility function.
- Never install packages from untrusted registries or arbitrary GitHub URLs without pinning to a specific commit hash.
- CI secrets (API keys, deploy tokens) must be stored in the CI platform's secret management — never in workflow files or Dockerfiles.
- CI runners should have least-privilege access — a build job doesn't need production database credentials.
- Never run arbitrary user-provided input as commands in CI (e.g. PR title in a shell script) — this is a command injection vector.
- If using Docker: never run containers as root in production, use multi-stage builds, scan images for CVEs, and never use `latest` tag for base images in production.
- Every deployment must verify the service is actually responding correctly after deploy — don't report success just because the process finished.
- Never deploy database migrations and application code simultaneously if the migration is breaking — deploy the migration first, verify, then deploy the code.

## What this rule set does not cover
This is prevention-focused. It does not replace: a real threat model for this specific app, monitoring/incident-response setup beyond the basics above, DDoS/WAF protection at the infra layer, or professional penetration testing. If the user asks for any of those, say so rather than pretending this checklist covers it.