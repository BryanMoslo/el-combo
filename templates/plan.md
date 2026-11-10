# <Title>

Issue: #N

## Request
<The need in the user's words, from the issue.>

## Research
**Current state**
- `path:line`: how it works today.

**Reusable pieces**
- Existing helpers, patterns, and components to build on.

**Constraints & risks**
- Security, performance, data, cost, external services.

## Decisions
<Each answer in the user's words, with the date. No open questions remain when the spec is written.>

## Spec
One PR into `<base branch>`, branch `<type>/<slug>`. (Stacked: PR 1, PR 2, … each from the previous one's branch.)

- [ ] **<deliverable>:** what changes, where, and how it behaves.

**Acceptance criteria**
- [ ] <A checkable statement.>

**Verify**
1. <A concrete check with a clear pass or fail.>

**Test cases**
- `tests/cases/<area>/<case>.md`: new | updated | reused.

**Database changes**
- Migrations, backfills, and the order they run in relative to the deploy. "None" if none.

**Environment variables**
- Each new or changed variable, what it's for, and where it must be set before merge. "None" if none.

**Other rollout steps**
- Cost caps, steps outside the repo. "None" if none.

## Build notes
<Behavior-neutral deviations from the spec, and why.>

## Verify notes
<Gates, test results with case ids, evidence files, fixes made, open findings, anything untested.>
