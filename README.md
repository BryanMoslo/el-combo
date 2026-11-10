# El Combo 🤹🏻‍♂️

A Product Owner, a Developer and a Tester for [Claude Code](https://code.claude.com), plus the phase commands that move work through them:

```
/el-combo:po  →  /el-combo:plan  →  /el-combo:build  ( →  verify  →  ship )
```

The agents are technology-agnostic. Each project tells them its gates, branches and conventions in its own `CLAUDE.md`, so the same three roles work in a Go service, a Rust app or a mobile client.

## The roles

| Agent | Stance | Owns |
|---|---|---|
| `el-combo:product-owner` | Strategic, technically critical | The why and the what: problem, scope, acceptance criteria, UX states, security and performance as requirements |
| `el-combo:developer` | Pragmatic, opinionated | The how: the smallest coherent change, reuse before new code, native before libraries, tests first. Says what it thinks, argues once, then follows the decision |
| `el-combo:tester` | Skeptical, thorough | The proof: the project's gates, the spec's checks, then edge cases, UX, performance, load, security and flaky networks, with evidence and reusable test cases |

Personality shows up in decisions, not in tone. None of them roleplays.

**Models.** Each role runs on the cheapest model that does its job well: the Product Owner on `opus` (effort `high`), because a wrong spec costs more than everything after it; the Developer and the Tester on `sonnet` (effort `medium`), which handle everyday coding and multistep tool work. Aliases track the current generation. To change one, edit `model` / `effort` in the agent's frontmatter; `CLAUDE_CODE_SUBAGENT_MODEL` doesn't override a model set there.

## The phases

| Command | What happens | Waits for you to |
|---|---|---|
| `/el-combo:po <need>` | The PO drafts the issue and revises it with you; once you approve, it's created with `gh` | approve the drafted issue before it's created |
| `/el-combo:plan <slug> #<issue>` | The PO researches and asks every open decision in one round, then writes the spec; the Tester checks the acceptance criteria; the plan is posted as a comment on the issue | answer the open questions |
| `/el-combo:build <slug>` | The Developer builds the spec with tests, then verify and ship run | answer anything the spec doesn't cover (it also stops if verify fails) |
| `/el-combo:verify` | The Tester runs the gates and exercises the change; the Developer reviews the diff | nothing; it reports and stops |
| `/el-combo:ship` | Commit, push, and a PR that closes the issue: what it adds, how it was tested, database changes, environment variables, evidence attached | confirm any decision not yet recorded |

Questions, commits and PRs always happen in the main conversation; the agents do the deep work and report back. Every phase writes to `.claude/plans/<slug>.md`, so the next phase, or a fresh session, picks up from the file instead of the chat.

## Install

In a Claude Code session:

```
/plugin marketplace add BryanMoslo/el-combo
/plugin install el-combo@el-combo
```

Or from your shell: `claude plugin marketplace add BryanMoslo/el-combo`, then `claude plugin install el-combo@el-combo`.

For a team, run this once in the project and commit the `.claude/settings.json` it writes; every teammate who trusts the folder gets the marketplace:

```
claude plugin marketplace add BryanMoslo/el-combo --scope project
```

To turn the plugin on for everyone too, add `"enabledPlugins": { "el-combo@el-combo": true }` to that same file.

**Updates** are off by default. Run `claude plugin update el-combo@el-combo`, or turn on auto-update for the marketplace under **Marketplaces** in `/plugin`.

## Adopt it in a project

1. Add an `## el-combo` section to the project's `CLAUDE.md` from [templates/project-contract.md](templates/project-contract.md): the gates command, base branch, how to run and log in locally, safe environments, unsafe actions, evidence rules, deploy.
2. Make the gates one command that exits non-zero on any failure (the same one CI runs).
3. Create `tests/cases/` (the Tester fills it as it goes) and `.claude/plans/`.
4. Put stack-specific rules (framework idioms, gotchas, architecture) in `CLAUDE.md` too. The agents follow them over their own defaults.

## What a project gets

- **Plans** in `.claude/plans/<slug>.md`, committed with the PR: request, research, your decisions in your words, the spec, build and verify notes. Template: [templates/plan.md](templates/plan.md).
- **Test cases** in `tests/cases/<area>/<case>.md`, committed and reused across features. Template: [templates/test-case.md](templates/test-case.md).
- **Issues** with the approved plan posted as a comment.
- **PRs** that close their issue and say what they add, how they were tested, database changes and environment variables, with evidence (screenshots, logs, timings) attached, never committed. Template: [templates/pr-body.md](templates/pr-body.md).
- **Quality expectations** that hold in any stack: [skills/quality/SKILL.md](skills/quality/SKILL.md).

## Ground rules

- No open decisions in a PR. Anything a user would notice is asked first.
- No "it passes" without output from this run.
- Never force-push, skip hooks, merge, release or deploy.
- No side-effect flows (real emails, payments, third parties) outside the safe environments the project names.
- No secrets, personal data or session links in commits, PRs or external requests.

## How this repo is organized, and why

The layout follows Anthropic's Claude Code plugin documentation (checked November 2026). What each choice rests on:

| Choice here | Best practice | Source |
|---|---|---|
| One repo that is both the plugin and its own marketplace: `.claude-plugin/plugin.json` plus `.claude-plugin/marketplace.json` with one entry whose `source` is `"./"` | Publishing from the plugin's own repository: the marketplace file sits beside `plugin.json`, its entry points at the repo root and has the same `name` as the manifest | [Publish and distribute a plugin](https://code.claude.com/docs/en/plugins/publish.md#publish-through-your-own-marketplace), [Create a marketplace](https://code.claude.com/docs/en/plugins/create-marketplace.md) |
| Components in the default folders at the root (`agents/`, `skills/`); only manifests inside `.claude-plugin/` | Standard layout: only the manifest goes in `.claude-plugin/` | [Plugin manifest reference: standard layout](https://code.claude.com/docs/en/plugins/manifest-reference.md#standard-layout) |
| Phases written as `skills/<name>/SKILL.md`, not `commands/` | Prefer `skills/` for new plugins | [Plugin manifest reference](https://code.claude.com/docs/en/plugins/manifest-reference.md#standard-layout) |
| No `CLAUDE.md` at the plugin root; every rule lives in an agent or a skill | A plugin-root `CLAUDE.md` isn't loaded as context | [Plugin manifest reference](https://code.claude.com/docs/en/plugins/manifest-reference.md#standard-layout) |
| A permanent kebab-case `name` (`el-combo`), never renamed | Users install by `name@marketplace`; a rename breaks every install (change `displayName`, or use a `renames` map) | [Publish: prepare for release](https://code.claude.com/docs/en/plugins/publish.md#prepare-your-plugin-for-release), [Host a marketplace: rename or remove](https://code.claude.com/docs/en/plugins/host-marketplace.md#rename-or-remove-a-plugin) |
| `version` set only in `plugin.json`, bumped on every release | Users get a new copy only when the version changes; never set it in both `plugin.json` and the marketplace entry | [Host a marketplace: release a new version](https://code.claude.com/docs/en/plugins/host-marketplace.md#release-a-new-version), [Versions and updates](https://code.claude.com/docs/en/plugins/loading.md#versions-and-updates) |
| `description`, `author`, `homepage`, `repository`, `license`, and a root `README.md` | The metadata users see before installing | [Publish: prepare for release](https://code.claude.com/docs/en/plugins/publish.md#prepare-your-plugin-for-release) |
| Templates referenced as `${CLAUDE_PLUGIN_ROOT}/templates/...` | Plugin files are referenced through `${CLAUDE_PLUGIN_ROOT}`, which skill and agent bodies substitute | [Plugin manifest reference: environment variables](https://code.claude.com/docs/en/plugins/manifest-reference.md#environment-variables) |
| `build` and `ship` set `disable-model-invocation: true`; `quality` and `test-cases` set `user-invocable: false` | Side-effect workflows run only when you type them; background knowledge stays out of the `/` menu | [Skills: invocation control](https://code.claude.com/docs/en/skills.md) |
| Each `SKILL.md` is short, with a description that leads with the use case | Keep `SKILL.md` under 500 lines; the description decides when a skill loads and is cut at 1,536 characters | [Skills](https://code.claude.com/docs/en/skills.md) |
| Agents set `model` and `effort`; the PO limits its `tools` | Supported subagent fields; plugin agents ignore `hooks`, `mcpServers` and `permissionMode` | [Subagents](https://code.claude.com/docs/en/sub-agents.md) |
| No top-level `bin/`, no Git LFS, no submodules | claude.ai rejects plugins with a top-level `bin/`; clones never download LFS content | [Host and maintain a marketplace](https://code.claude.com/docs/en/plugins/host-marketplace.md) |

| `evals/` with one folder per case (`prompt.md` plus `graders/`), results git-ignored | Test plugin behavior with `claude plugin eval`; pair a grader on the result with one on the steps that produced it; prefer free graders (`regex`, `tool_used`) and keep `llm` rubrics short, with concrete PASS/FAIL conditions | [Test plugins with evals](https://code.claude.com/docs/en/plugin-evals.md) |
| CI runs `claude plugin validate --strict .` on every push; evals run locally only | Keep `--strict` in CI. Evals in CI would need an API key billed per use, so they stay on your machine | [Publish: prepare for release](https://code.claude.com/docs/en/plugins/publish.md#prepare-your-plugin-for-release), [Evals: requirements](https://code.claude.com/docs/en/plugin-evals.md) |

## Evals

`evals/` holds six cases. They check the behavior the plugin promises, not its wording:

| Case | Tag | Checks |
|---|---|---|
| `build-needs-a-spec` | smoke | `/el-combo:build` without a plan points to `/el-combo:plan` and starts no developer |
| `ship-needs-verify` | smoke | `/el-combo:ship` without a green verify offers verify and opens no PR |
| `ignores-unrelated-request` | smoke | A general question loads none of the plugin's skills |
| `plan-needs-an-issue` | smoke | A plan request with no issue asks for the issue (or offers `/el-combo:po`) and writes no spec |
| `tester-finds-gaps` | agents | The tester names the missing states and risks in thin acceptance criteria |
| `developer-flags-overengineering` | agents | The developer argues against a single-implementation abstraction |

Run them from the repo root. They use your own Claude login, so with a subscription they count against your plan's usage limits like any other session, with no extra charge. The `$` figures the command prints are list-price estimates, not a bill:

```
claude plugin eval . --tag smoke
claude plugin eval .
```

By default each case runs 3 times with the plugin and 3 times without it, and reports the difference. Add `--ablation none` to skip the no-plugin runs, or `--runs 1` while iterating.

## CI

`.github/workflows/validate.yml` runs `claude plugin validate --strict .` on every push and pull request. It needs no credentials and doesn't call a model.

## Releasing

1. Bump `version` in `.claude-plugin/plugin.json` (semver).
2. Run `claude plugin validate --strict .`; it must print `✔ Validation passed`. Run `claude plugin eval .`; every case must pass.
3. Add the checkout as a local marketplace (`claude plugin marketplace add ./`), install from it, and check that it loads.
4. Commit and push to `main`, then run `claude plugin tag --push`, which checks that `plugin.json` and the marketplace entry agree and creates the `el-combo--v<version>` tag that version ranges resolve against.

## License

[MIT](LICENSE)
