---
name: verify
description: Testing and review phase. Runs the project's gates, a developer review of the diff, and a tester pass that exercises the change for real, with evidence and reusable test cases. Use when the user says "verify", "test it", "check it", or "is it ready".
argument-hint: [slug]
---

# Verify: $ARGUMENTS

1. Read the project's CLAUDE.md and its `## el-combo` contract. Find the plan file (`.claude/plans/<slug>.md`) from the argument or the branch name. No **Gates** command in the contract → ask the user for it once.
2. In parallel:
   - the `el-combo:tester` agent: run the **Gates** command and report each check with pass/fail and exact errors; then, when the change has UI or runtime behavior, run the spec's **Verify** checks and explore beyond them, writing or updating `tests/cases/` and collecting evidence;
   - the `el-combo:developer` agent, as reviewer: the diff against the base branch plus uncommitted changes, with the spec as context.
3. Never report a gate as passing without its output from this run.
   - fix failures this change caused, and CONFIRMED findings that are clearly in scope, then re-run what they affect;
   - report pre-existing failures; don't fix them silently;
   - list the remaining findings as open.
4. Append `## Verify notes` to the plan file: gates, test results with case ids, evidence file paths, fixes made, open findings, anything untested.
5. Keep the evidence files in the scratchpad for `/el-combo:ship`; never commit them.
6. Reply: gates table, test results table, review findings (fixed / open), anything untested. Suggest `/el-combo:ship` only if green. Stop.
