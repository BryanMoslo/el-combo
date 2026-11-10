---
name: test-cases
user-invocable: false
description: Format and rules for reusable test cases in tests/cases/ and for the evidence attached to PRs. Use when writing, updating or running a test case, or when collecting evidence.
---

# Test cases

Cases live in the project at `tests/cases/<area>/<case>.md`, are committed, and are reused across features. Template: `${CLAUDE_PLUGIN_ROOT}/templates/test-case.md`.

## Rules
- **One behavior per case.** Variants (edge cases of the same behavior) go in the case's **Variants** list, not in new files.
- **Behavior, not implementation.** Write what a person does and sees, so the case survives refactors. No function names, selectors, or file paths in the steps.
- **Reuse first.** Search `tests/cases/` (by `area`, then by text) before writing a new case. Extend a matching case with a variant rather than duplicating it.
- **Ids are stable.** `id` is `<area>-<short-name>`, kebab-case, and never changes once it's referenced from a plan or PR. A case that no longer applies gets `status: retired`; it isn't deleted.
- **Record every run.** Update `last_run` (date, result, PR or branch) each time the case runs. A failing run also records the failure in the plan's `## Verify notes`.
- **Data and safety.** Preconditions say what data is needed and how to create it through the product. Steps never trigger the contract's unsafe actions outside its safe environments.

## Types
`functional` · `ux` · `performance` · `load` · `security` · `network`. One per case; a behavior with a performance angle gets a separate `performance` case.

## Priority
`p1`: a failure blocks shipping. `p2`: a failure needs a decision from the user. `p3`: a failure is noted and can ship.

## References
- **Plan:** the spec's **Test cases** list links each case it relies on, marked `new`, `updated`, or `reused`.
- **PR:** the `## Test results` table lists each case run: id, result, notes, and the evidence captions.

## Evidence
- Never committed. Saved to the scratchpad and attached to the PR (`gh pr create --attach` / `gh pr edit --attach`).
- File name: `NN-<case-id>-<state>.png` (or `.txt`/`.log` for logs and timings).
- Caption: the case id, the state, and the theme, e.g. `checkout-empty-cart, empty state (light)`.
- Screenshots: one full screen per state at the contract's viewport. No lone crops. Dark mode only when visuals changed.
- Non-visual evidence counts: request/response pairs, timings, log lines, query counts. Strip secrets and personal data first.
