---
name: ship
disable-model-invocation: true
description: Ship phase. Commit the change, push the branch, and open the PR with test results and evidence attached. Use when the user says "ship", "open the PR", or after a green verify inside /el-combo:build.
argument-hint: [slug]
---

# Ship: $ARGUMENTS

1. Refuse on the contract's base or release branch, or if `/el-combo:verify` hasn't passed in this session (offer to run it).
2. Every decision the change rests on is confirmed under `## Decisions` in the plan file. Anything unconfirmed → stop and ask. The PR body never has a "decisions to confirm" or open-questions section.
3. `git status` and the diff. Stage only files belonging to this change: code, tests, `tests/cases/` files, and the plan file. Never stage secrets, `.env` files, binaries, or evidence.
4. Commit: small and focused, imperative subject of 60 characters or fewer, a body that explains the why when it isn't obvious. Never skip hooks.
5. Evidence (when the change has UI or runtime behavior; skip for tooling-only changes):
   - captured from the committed code, per the contract's **Evidence** rules;
   - name files `NN-<case-id>-<state>.png` in the scratchpad;
   - attach with `gh pr create --attach '<file>#<caption>'` (or `gh pr edit <n> --attach` later). Check support first with `gh pr create --help | grep -q -- --attach`; missing → stop and ask the user to upgrade `gh`. Never fall back to committing images.
6. Push the branch (never force-push). Open the PR into the contract's base branch (the release branch only for a hotfix the user named). Body from `${CLAUDE_PLUGIN_ROOT}/templates/pr-body.md`, every section filled (write "None" where nothing applies): `Closes #N` for the plan's issue, what it adds, how it was tested (gates and test cases with results, from `## Verify notes`), database changes and environment variables (from the spec), and `## Evidence` last, so the attachments land under it. Captions name the case id, the state, and the theme.
7. Reply with the PR link and the deploy path from the contract. Never merge, release, or deploy.

After a fix on an open PR: attach new evidence for the fixed states and say which earlier attachments are outdated.
