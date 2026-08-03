You are a coding agent that must follow disciplined version control practices. Apply these rules to every commit, branch, and push you make in this project.

## Core directive
Git history is a safety net and a record of truth. Never take an action that destroys history or bypasses review just because it's faster in the moment.

## 1. Commits
- Write commit messages that describe *why*, not just *what* — "fix login redirect loop after logout" not "fix bug."
- Keep commits scoped to one logical change — don't bundle an unrelated refactor into a feature commit.
- Never commit secrets, `.env` files, API keys, or credentials. If one is accidentally staged, stop and flag it — don't just unstage it silently, since it may need to be rotated if already exposed.
- Never commit commented-out dead code or debug `console.log`/`print` statements left in by accident — clean up before committing.
- Never commit generated files (build output, `node_modules`, `.next`, `dist`) unless the project explicitly requires it — verify `.gitignore` covers them.

## 2. Branching
- Never commit directly to `main`/`master` for anything beyond a trivial fix the user explicitly approved — use a feature branch.
- Name branches descriptively (`fix/login-redirect`, `feat/user-profile-page`) — not `patch1` or `temp`.
- Don't let a branch drift indefinitely without merging — flag long-lived branches back to the user rather than letting them silently diverge from main.
- Delete merged branches to keep the repo clean — stale branches create confusion about what's active.

## 3. Force-Push & History Rewriting
- **Never force-push (`git push --force`) to `main`/`master` under any circumstances**, even to "fix" something — this can destroy other people's work with no recovery.
- Force-push to a personal feature branch only if the user explicitly confirms no one else is working on it.
- Never use `git reset --hard` on shared branches without explicit confirmation — it can silently discard committed work.
- Never rewrite published/shared history (`rebase`, `filter-branch`) without the user's explicit go-ahead.

## 4. Merging & Review
- Never merge your own feature branch into `main` without the user reviewing it first, unless they've explicitly said to auto-merge for this project.
- Prefer pull requests over direct merges when the tooling supports it — even solo, PRs create a review checkpoint and a diff to actually look at before it lands.
- Before merging, confirm the branch is up to date with `main` and there are no unresolved conflicts left half-fixed.
- Resolve merge conflicts carefully — never accept "both" sides without reading the conflict to understand what each side intended.

## 5. Undo Safety
- Before any destructive git operation (hard reset, force-push, branch delete), state what will be lost and get explicit confirmation — don't assume "the user probably means this."
- If something goes wrong, don't try to silently patch over it — tell the user what happened and what the recovery options are (reflog, backup branch, etc.).

## 6. .gitignore Hygiene
- Every project must have a `.gitignore` that covers: environment files (`.env`, `.env.*`), dependency directories (`node_modules`, `venv`, `__pycache__`), build output (`dist`, `.next`, `build`), IDE config (`.idea`, `.vscode/settings.json`), and OS files (`.DS_Store`, `Thumbs.db`).
- Before the first commit of a new project, verify `.gitignore` is set up — don't commit the first time and then add it after the damage is done.
- If a file that should be ignored is already tracked, remove it from tracking (`git rm --cached`) and add it to `.gitignore` — don't just add the ignore rule and leave the tracked file in history.

## What this rule does not cover
This is about git process discipline, not code review quality or CI/CD pipeline configuration — see backend_rules.md and agent_rules.md for what must be true about the code itself before it's committed.