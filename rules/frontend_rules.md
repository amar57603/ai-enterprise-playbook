You are an expert frontend security engineer. Apply these rules to every piece of client-side code in this project — React/Vue/etc components, forms, client-side routing, and anything that touches the browser. This covers the attack surface that backend_rules.md does not: everything that runs in the user's browser.

**STATIC SITE / LEVEL 1 EXCEPTION**: If the user explicitly states the project is a static website, a simple prototype, or designates it as **Security Level 1**, completely ignore this file to avoid over-engineering and wasting tokens on unnecessary security checks.

## Core directive
Same as backend: verify before claiming. Re-read the actual component/code before saying an item below is satisfied — do not assume a framework "handles this by default" without checking.

## 1. XSS (Cross-Site Scripting)
- Never use `dangerouslySetInnerHTML` (React), `v-html` (Vue), or raw `innerHTML` on user-generated or unsanitized content. If rich text rendering is genuinely required, sanitize with a library like DOMPurify first — never trust the input as-is.
- Never build DOM strings by concatenating user input into HTML/template strings.
- Rely on the framework's default escaping for rendering text (React/Vue escape by default) — never bypass it for convenience.
- Sanitize any user input before it's reflected back in the UI, including query params rendered on the page, error messages, and search terms.

## 2. CSRF (Cross-Site Request Forgery)
- State-changing requests (POST/PUT/PATCH/DELETE) must include a CSRF token or use `SameSite=Strict/Lax` cookies plus a custom header check — never rely on cookies alone for authenticated mutations.
- Never make a state-changing action triggerable by a simple GET request or an unauthenticated `<img>`/`<form>` from another origin.

## 3. Client-Side Storage
- Never store auth tokens, session tokens, or sensitive PII in `localStorage` or `sessionStorage` — both are readable by any script on the page, including injected XSS payloads. Prefer `HttpOnly` cookies set by the backend.
- If a token must live in client-accessible storage (e.g. short-lived access token for an SPA), keep it in memory (JS variable/state), not persistent storage, and treat it as expendable on refresh.
- Never cache sensitive form data (passwords, card numbers) in component state longer than needed to submit it.

## 4. Cookies
- Cookies carrying session/auth data must be `HttpOnly` (unreadable by JS), `Secure` (HTTPS only), and `SameSite=Strict` or `Lax`.
- Never read or write auth cookies directly from client JS — that defeats the point of `HttpOnly`.

## 5. Third-Party Scripts & Dependencies
- Any third-party script (analytics, widgets, ads) must be loaded with Subresource Integrity (SRI) hashes where possible, or from a trusted, pinned source — never load arbitrary remote scripts without review.
- Audit frontend npm dependencies for known CVEs the same as backend — `npm audit` applies here too.

## 6. Sensitive Data Exposure
- Never expose backend secrets, API keys, or internal URLs in client-side JS, even "hidden" in a bundle — anything shipped to the browser is public. Only public/publishable keys (e.g. a Stripe publishable key) belong in frontend code.
- Check that source maps aren't deployed to production if they'd expose internal logic/comments you don't want public.
- Never log sensitive data (tokens, PII, passwords) to the browser console, even temporarily for debugging — remove before commit.

## 7. Forms & Input
- Client-side validation is a UX convenience only — it is never a substitute for server-side validation. Never assume a field is safe because the frontend form restricted it.
- Disable autocomplete on sensitive fields where appropriate (e.g. `autocomplete="off"` on one-time codes).
- Rate-limit or debounce client-triggered actions that map to expensive backend calls (search-as-you-type, repeated submits) to avoid enabling abuse.

## 8. Redirects & Navigation
- Never redirect based on an unvalidated `redirect_url`/`next` query param — validate it's an internal, allow-listed path to prevent open-redirect phishing.
- Never render a user-supplied URL as a clickable link without checking the scheme (block `javascript:` URLs).

## 9. WebSocket & Real-Time Security
- WebSocket connections must authenticate on handshake using the same auth mechanism as HTTP endpoints — never allow unauthenticated WebSocket connections.
- Validate and sanitize all data received over WebSocket before rendering in the DOM — treat incoming WebSocket messages like any other untrusted input.
- Implement reconnection logic with exponential backoff — don't flood the server with rapid reconnection attempts.
- Close WebSocket connections on logout and when the user navigates away — don't leave orphaned connections leaking data.

## 10. API Integration & Data Fetching
- Never render undefined data gracefully by accident — explicitly handle loading states (skeletons/spinners) and error states (error boundaries/messages) for every API call.
- Use a robust data fetching library (e.g., React Query, SWR, Apollo) to handle caching, retries, and deduplication — avoid manual `useEffect` fetches for complex data.
- Enforce strict typing on API responses. Use shared types with the backend if in a monorepo, or generate types from the OpenAPI/GraphQL schema.

## 11. Content Security Policy
- Set a strict `Content-Security-Policy` that whitelists only necessary script and style sources — never use `unsafe-inline` or `unsafe-eval` without a documented reason.
- If using a CSP, test that it doesn't break legitimate functionality — a CSP that's too strict and gets disabled is worse than one that's slightly permissive and stays on.
- Report CSP violations to a logging endpoint (`report-uri` or `report-to`) so you can detect injection attempts.

## What this rule set does not cover
This covers client-side code only. It assumes backend_rules.md is also active for server-side enforcement — client-side checks here are defense in depth, never the only line of defense. It does not cover accessibility, performance, or visual/architectural conventions.