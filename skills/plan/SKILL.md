---
name: plan
description: Planning phase. Research a feature, bug or request, ask every open decision in one round, and write a spec ready for approval. Use when the user says "plan", "research", "spec", "investigate", or "how should we build".
argument-hint: <slug> #<issue>
---

# Plan: $ARGUMENTS

Goal: `.claude/plans/<slug>.md` with a spec and **no open decisions**. **Do not edit source code.**

1. Read the project's CLAUDE.md and its `## el-combo` contract. Plan file already exists → continue from it.
2. The plan belongs to a GitHub issue. Read it with `gh issue view <n> --comments`. No issue named → ask for its number, or offer to run `/el-combo:po` first. Create the plan file from `${CLAUDE_PLUGIN_ROOT}/templates/plan.md` if it doesn't exist, with the issue number and the request.
3. Delegate to the `el-combo:product-owner` agent: the slug, the request, the plan file path, and every decision already made (in the plan file or in this conversation). It writes `## Research` and returns open questions. Designs named in the contract count as decided where they're explicit.
4. Ask every open question with `AskUserQuestion` in one round (batch related ones, recommendation first). Record each answer under `## Decisions` in the user's words, with the date. Follow-up questions an answer raises get asked too.
5. Send the agent back to write `## Spec`. Repeat steps 4–5 until it returns no questions.
6. Delegate to the `el-combo:tester` agent to review the acceptance criteria: gaps, uncheckable criteria, missing states, and which `tests/cases/` apply or need writing. Gaps that change behavior → step 4; the rest go into the spec's **Test cases** list.
7. Read the spec against `## Decisions` and the request yourself:
   - fix anything that contradicts a decision;
   - remove anything nobody asked for, or ask about it;
   - any "default", "assumed", "TBD", or "my call" left → step 4.
8. Nothing open → post the plan file as a comment on the issue: `gh issue comment <n> --body-file .claude/plans/<slug>.md`. A plan was posted there before (a re-plan) → post the new one starting with "Updated plan; replaces the earlier one." Never edit or delete earlier comments.
9. Reply with the checklist, the decisions it rests on, and the comment link. Suggest `/el-combo:build <slug>` only when nothing is open. Stop.
