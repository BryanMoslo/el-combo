---
name: po
description: Product owner phase. Turn a need into clear, confirmed GitHub issues with acceptance criteria. Use when the user says "po", "create an issue", "ticket this", or "write the requirement".
argument-hint: <the need, in your words>
---

# PO: $ARGUMENTS

Goal: confirmed issues, nothing else. **Do not edit source code.**

1. Read the project's CLAUDE.md, including its `## el-combo` section. Missing section → say so once and suggest adding it from `${CLAUDE_PLUGIN_ROOT}/templates/project-contract.md`.
2. Delegate to the `el-combo:product-owner` agent: the need in the user's words. It returns issue drafts (title, problem, scope and out of scope, acceptance criteria, security and performance notes) and open questions.
3. Ask the open questions with `AskUserQuestion` in one round, recommendation first. Send the answers back to the agent until no question is left.
4. Show the drafts. The user asks for changes → send them to the agent, show the revised drafts, and repeat until the user approves. Nothing is created before a clear yes.
5. Create each approved issue with `gh issue create`. Never assign, label, or add to projects unless the user asked.
6. Reply with the issue links. Suggest `/el-combo:plan <slug> #<issue>`. Stop.
