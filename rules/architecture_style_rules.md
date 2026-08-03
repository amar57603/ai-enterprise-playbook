You are a coding agent that must keep this codebase structurally consistent as it grows. Apply these rules to how you organize files, name things, manage state, and handle cross-cutting concerns — across both frontend and backend.

## Core directive
Consistency compounds. A pattern picked in week one and never revisited becomes unmaintainable by week ten if every feature invents its own structure. Follow the existing pattern in the codebase; only introduce a new one with explicit reasoning, and flag it to the user rather than silently diverging.

## 1. Folder Structure
- Group by feature/domain, not by file type, unless the project has already established a type-based structure (e.g. `components/`, `hooks/`, `utils/` at top level) — match whatever exists, don't mix both patterns in the same project.
- Keep a consistent depth — don't nest a new feature 6 folders deep when everything else is 2-3 deep.
- Shared/reusable code goes in a clearly named shared location (`shared/`, `common/`, `lib/`) — never duplicate a utility function in multiple places because it was faster than finding the existing one.
- Test files follow one consistent convention across the project (co-located `*.test.ts` next to source, or a mirrored `__tests__/` tree) — pick one, don't mix.

## 2. Naming Conventions (Strict)
- **Files:** Use strictly one casing convention based on the context. E.g., `kebab-case.ts` for utilities/configs, `PascalCase.tsx` for React components. Do not mix cases.
- **Functions:** Use descriptive `camelCase` verbs (e.g., `fetchUserData`, `calculateTotalTax`). Never use vague placeholders like `handleData`, `doStuff`, `utils2`, or single-letter variables `x`, `y` (except in short math loops).
- **Booleans:** Must read as yes/no questions (`isLoading`, `hasError`, `canSubmit`, `shouldFetch`). Never use just `loading` or `error` which are ambiguous.
- **Constants:** Use `UPPER_SNAKE_CASE` for global constants (`MAX_UPLOAD_SIZE`).
- **Terminology:** Stick to one term for the same concept across the entire app. If you call it `User` in the database, don't call it `Account` in the UI.

## 3. State Management
- Match the state management pattern already established in the project (Context, Redux, Zustand, plain hooks, etc.) — don't introduce a second state library because it's more familiar or convenient for one feature.
- Keep state as local as possible; only lift to global/shared state when multiple unrelated components genuinely need it — don't default to global state for convenience.
- Never duplicate the same piece of state in two places (e.g. local component state that's also in a global store) without a clear, stated reason — this is a common source of subtle bugs where the two get out of sync.
- Derived values (computed from other state) should be computed, not stored — don't add a new state variable for something that can be calculated from existing state.

## 4. Component Design (Frontend)
- Keep components focused — if a component handles data fetching, business logic, and rendering all at once with no separation, flag it as a candidate to split, don't just keep adding to it.
- Extract repeated JSX/markup patterns into a shared component once they appear 2-3+ times — don't copy-paste the same block across files.
- Props should be minimal and explicit — avoid passing an entire object down when only 2 of its fields are used, unless that's the established pattern in this codebase.

## 5. Error Handling Patterns
- Use one consistent error handling pattern across the app — don't mix try/catch in some places, `.catch()` in others, and error boundaries in others without a clear rationale for each.
- Define a standard error shape that flows through the app (e.g. `{ code, message, field? }`) — don't ad-hoc error objects per feature.
- Every async operation (API call, file read, external service) must handle the failure case explicitly — never leave a promise with no catch or an async/await with no try/catch.

## 6. Code Reuse & DRY
- Before writing a new utility function, check if one already exists in the codebase — duplicate utilities are the most common form of accidental inconsistency.
- Shared logic used by 3+ features belongs in a shared module, not copy-pasted — but don't prematurely abstract one-off code into a shared utility after its first use.
- Configuration values used in multiple places (API base URLs, feature flags, magic numbers) should be defined once in a config/constants file — not scattered as string literals across the codebase.

## 7. Type Safety
- If the project uses TypeScript, use it properly — no `any` types as a shortcut unless genuinely unavoidable, and document why.
- Shared data shapes (API responses, database models, form values) should have defined types/interfaces used consistently — don't re-define the same shape inline in multiple files.
- Function signatures should have explicit parameter and return types for anything used across modules — internal helpers can rely on inference.

## 8. Testing Quality & Strategy
- Use Test-Driven Development (TDD) where applicable: write the test for critical business logic before the implementation.
- Maintain a pyramid of tests: Unit tests for pure logic/utils, Integration tests for API routes/database interactions, and End-to-End (E2E) tests (e.g., Playwright, Cypress) for critical user journeys.
- Never mock the database for integration tests if you can avoid it — use a test database (e.g., testcontainers) to catch real SQL/ORM errors.
- Tests should assert behavior, not implementation details. Don't test that a function was called 3 times, test that the final output is correct.

## 9. Frontend & Backend Decoupling (No Cross-Bugs)
- **API Contracts:** The frontend and backend must communicate strictly through a defined API contract (e.g., OpenAPI/Swagger or GraphQL schema). Do not tightly couple internal backend logic to the frontend UI.
- **Backward Compatibility:** The backend must NEVER introduce a breaking change to an existing API endpoint without versioning it first. If you need to rename a field, add the new field alongside the old one, update the frontend to use the new one, and only then deprecate the old one.
- **Independent Deployability:** You must be able to deploy a backend update without breaking the frontend, and deploy a frontend update without requiring a backend restart. 
- **Mocking:** The frontend should be able to run and be tested using a mock API (e.g., MSW - Mock Service Worker) so frontend development doesn't grind to a halt if the backend is down or undergoing changes.
- **Clear Boundaries:** Do not share internal utility functions or raw database models directly with the frontend code. Map database models to separate Data Transfer Objects (DTOs) before sending them over the network.

## 10. Clean Code, Comments & Handover
- **Mandatory Documentation:** Every major function, class, and component MUST have a standard docstring (e.g., JSDoc for JS/TS, Docstrings for Python) explaining what it does, what its parameters are, and what it returns. This ensures the codebase can be easily updated in the future.
- **Explain the "Why":** Code explains *what* is happening. Comments must explain *why* it's happening (e.g., "We use a 500ms debounce here to prevent rate-limiting from the payment API").
- **No Dead Code:** Never leave commented-out blocks of old code, unused imports, or `console.log`s in the final version. It must be pristine.
- Maintain a pristine `README.md` at the project root: it must include setup instructions, environment variables required, and how to run tests.
- Auto-generate API documentation (e.g., Swagger/OpenAPI for REST, GraphQL Playground).
- Document any manual operational tasks (e.g. "how to rotate the DB password") in a runbook or architecture document.

## 11. Consistency Checks Before Marking Work Done
- Before finishing a feature, check: does this match the existing folder structure, naming, state pattern, and error handling in the rest of the codebase — or did I just invent something new because it was faster? If you diverged, say so explicitly and explain why, rather than letting it blend in silently.

## What this rule does not cover
This is about code organization and consistency, not visual/design decisions (see design_direction_rules.md), not security (see backend_rules.md / frontend_rules.md), and not git process (see git_discipline_rules.md).