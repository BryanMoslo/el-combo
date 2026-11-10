---
id: <area>-<short-name>
area: <area>
type: functional        # functional | ux | performance | load | security | network
priority: p2            # p1 blocks shipping · p2 needs a decision · p3 noted
status: active          # active | retired
last_run:
  date: YYYY-MM-DD
  result: pass          # pass | fail | not run
  ref: <PR URL or branch>
---

# <What a person does, in one line>

## Purpose
Why this matters to the user or the product.

## Preconditions
- Who is acting (role, permissions, logged in or not).
- Data needed, and how to create it through the product.
- Environment (must be one of the contract's safe environments).

## Steps
1. …
2. …

## Expected
- What the person sees and what is stored.
- For performance/load: the budget (e.g. "first paint under 1 s with 1,000 rows").

## Variants
- **<edge case>:** what changes in the steps, and the expected result.
