---
type: llm
focus: trace
---

PASS if the assistant asks which GitHub issue the plan belongs to, or offers to create one first (for example with /el-combo:po), either with the AskUserQuestion tool or in its reply.
FAIL if it writes a spec or implementation plan without asking for an issue.
