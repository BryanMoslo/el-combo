---
name: build
disable-model-invocation: true
description: Implementation phase, then verify and ship in one run, for every PR of the spec. Executes an approved spec (or a small direct request) with tests, verifies it, and opens the PR. Use when the user says "build", "implement", "do it", or "code it".
argument-hint: <slug>
---

# Build: $ARGUMENTS

Asking for `/el-combo:build` is asking to commit, push, and open the PR once verify is green.

1. Read the project's CLAUDE.md and its `## el-combo` contract. `.claude/plans/<slug>.md` has a `## Spec` → follow it. No spec and the task isn't a small, obvious change → stop and suggest `/el-combo:plan`.
2. Check the branch. On the base or release branch → fetch and create `<type>/<slug>` (or the contract's branch naming) from the remote base branch. Another session uses this checkout → work in your own worktree. The branch already has a PR → check it's still open; merged → a new branch and PR.
3. Delegate to the `el-combo:developer` agent: the plan file path and which checklist items to build. It writes failing tests first, implements, runs the narrowest tests, ticks items, and writes `## Build notes`. It returns questions and the deviations it took.
4. A question back from the developer that changes what a user would see or what the product does → ask with `AskUserQuestion`, record the answer under `## Decisions`, and send the developer on. The developer disagreed with the spec → put its argument and trade-off to the user once; the user decides.
5. Self-review the diff against the base branch: every spec item is in it or explained in Build notes, nothing outside the spec is in it, tests ran fresh in this session.
6. Read `${CLAUDE_PLUGIN_ROOT}/skills/verify/SKILL.md` and follow its steps 1–5. Its "stop" doesn't apply here.
7. Verify not green (a gate this change broke, or a confirmed in-scope finding left open) → stop and report. Don't ship.
8. Green → read `${CLAUDE_PLUGIN_ROOT}/skills/ship/SKILL.md` and follow it. It still stops for any unconfirmed decision.
9. Spec has more PRs (stacked) → repeat from step 2 for the next one, branching from the previous PR's branch.
10. Reply once at the end: PR links, the gates table, review findings (fixed / open), test results, deviations, anything untested, and the deploy path from the contract.
