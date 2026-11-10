---
name: quality
user-invocable: false
description: Universal quality expectations for any change in any stack, which every phase checks against. Use when writing, specifying, reviewing or testing a change. Language and framework rules come from the project's CLAUDE.md, not from here.
---

# Quality expectations

These hold in every project. The project's mechanical gates (its **Gates** command) prove some of them; the rest are checked by the developer's review and the tester's pass. Project rules in CLAUDE.md add to these and win on conflict.

## Correctness
- Every behavior change has a test that fails without it.
- Every gate is green before shipping, with output from this run.
- Root causes are fixed; symptoms aren't patched over or guarded twice.
- No silent data loss: destructive operations are confirmed, reversible, or recoverable; migrations add before they remove.

## Security
- No secrets in code, logs, commits, PRs, or URLs.
- Untrusted input is validated at the boundary; untrusted content is escaped or sanitized before it's rendered.
- Authorization is checked on the server for every action, not only hidden in the UI.
- Queries on user data are scoped to the owner in the query itself.
- Errors shown to users don't leak internals.

## Reliability and performance
- External calls have timeouts, handle failure, and respect rate limits and cost caps.
- No query or call per row where one batch would do.
- Work that grows with data has a known bound.
- Retries are safe to repeat (idempotent) or guarded.

## UX and accessibility
- Every state is designed: empty, loading, error, denied, 0 / 1 / many, long content.
- Inputs have labels; focus is visible and lands where expected; it works with a keyboard alone.
- Text contrast is readable in every theme the product supports.
- Copy is plain and specific; errors say what to do next.
- After a write, what the user sees matches what was stored (a reload shows the same thing).

## Simplicity and maintainability
- The smallest coherent change: reuse what exists, extend the layer that owns it, prefer native features over new dependencies.
- No speculative abstractions, configuration for fixed values, or single-implementation interfaces.
- Code matches its surroundings; comments explain only a non-obvious why.

## Process
- No open decisions in a PR: every choice a user would notice was asked and recorded.
- Commits are small and focused; nothing unrelated rides along.
- Never force-push, skip hooks, merge, release, or deploy from a phase.
- No personal data, secrets, or session links in commits, PRs, or requests to external services.
