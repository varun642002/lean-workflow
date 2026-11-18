<div align="center">

# lean-workflow

**Token-lean agents and workflow skills for [Claude Code](https://claude.com/claude-code).**

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Plugin](https://img.shields.io/badge/Claude%20Code-plugin-5A4FCF.svg)](https://docs.claude.com/en/docs/claude-code/plugins)
[![Agents](https://img.shields.io/badge/agents-10-blue.svg)](#agents)
[![Skills](https://img.shields.io/badge/skills-6-blue.svg)](#skills)
[![Free](https://img.shields.io/badge/free-forever-brightgreen.svg)](#license)

</div>

---

## Why

A general-purpose agent asked to fix a bug will read whole files, summarise what it read, explain its plan, and narrate every edit. Most of that is context you pay for and never use.

Every agent in this plugin is written against the same three constraints:

1. **Locate before reading** — `grep -n` for the symbol first, then read only the matched range.
2. **Never read a large file in full** — anything over 300 lines is read by line range.
3. **Report in a fixed, capped format** — `file:line — problem — fix`, with a hard line limit and no preamble, no praise, no summary of what the diff already shows.

The outcome is the same plan → build → verify → review loop at a fraction of the context, with output short enough to actually read.

## Install

Free, public and open source. No account, no key, no telemetry — the whole plugin is Markdown and JSON you can read in a minute.

```
/plugin marketplace add varun642002/lean-workflow
```

```
/plugin install lean-workflow@lean-workflow
```

Restart Claude Code and the six skills and ten agents are available in every project.

## Skills

Each skill orchestrates the agents below, so you describe intent and nothing else.

| Command | What it does |
|---|---|
| `/feature <what to build>` | Plans with `lean-planner`, implements it (parallel `lean-builder`s for independent parts), then tests and reviews. Skips planning when the change is one or two obvious files. |
| `/fix <symptom>` | Finds the root cause with `lean-debugger`, applies the minimal fix, verifies it. No unrelated changes. |
| `/debug <symptom>` | Investigation only. Routes to the UI, logic or performance debugger and stops before touching anything. |
| `/secure [area \| "test"]` | `lean-security` audit of a diff or an area. Pass `test` to also probe the running localhost app with `lean-security-tester`. |
| `/review [commit \| branch]` | `lean-reviewer` and `lean-tester` in parallel. Bug-only findings, no style nits. |
| `/ship [pr]` | Type check, branch if on main, commit in the repository's own message style, optionally open a PR. |

Nothing is committed and no fix is applied without an explicit yes.

## Agents

| Agent | Model | Writes | Purpose |
|---|---|---|---|
| `lean-planner` | inherit | — | Numbered plan naming every file and function to touch, before any change over two files. |
| `lean-builder` | sonnet | source | Implements one well-defined step. Run several in parallel, one per file group, never two on a file. |
| `lean-debugger` | inherit | — | Back-end and logic bugs: wrong totals, failed saves, API errors, stack traces. |
| `lean-ui-debugger` | inherit | — | Front-end bugs. Traces component → state → data → styling and stops when proven. |
| `lean-perf` | sonnet | — | Measured bottlenecks: queries in loops, unbounded reads, re-renders, sequential awaits, bundle weight. |
| `lean-security` | sonnet | — | Auth, authorization, database rules, injection, secrets, PII exposure, client-supplied prices and totals. |
| `lean-security-tester` | inherit | — | Actively probes the running app. Localhost only, non-destructive, never prints a secret it finds. |
| `lean-reviewer` | sonnet | — | Bug-only diff review. Up to eight findings, each with a concrete failing case. |
| `lean-tester` | haiku | — | Type check plus tests, failures only. The cheapest possible verification step. |
| `lean-docs` | sonnet | docs | README, setup steps, API reference, env vars. Restricted to `*.md`, `docs/**`, `.env.example` and doc comments. |

Only `lean-builder` can change source code and only `lean-docs` can change documentation. Every other agent is read-only by construction, not by instruction.

## Optional: automatic routing

To have Claude pick the right agent from a plain sentence, with no slash command typed, add this to your `CLAUDE.md`:

```markdown
| Request | Agent |
|---|---|
| New feature / change over 2+ files | lean-planner, then lean-builder (parallel for independent parts) |
| Logic, data, API or server bug; stack trace | lean-debugger |
| Screen/UI bug, layout, React state, console error | lean-ui-debugger |
| Slow page, API, DB cost, bundle size | lean-perf |
| Security question; change to auth, roles, rules, API, payments, uploads | lean-security |
| "Security test" / pentest / verify a security fix (localhost only) | lean-security-tester |
| Docs out of date; README, setup steps, API reference, env vars | lean-docs |
| After any code change | lean-tester + lean-reviewer (in parallel) |
```

## Compatibility

Stack-agnostic. The security, performance and UI agents name Firestore and React because that is what they were written against; their checklists skip whatever does not apply, and the token rules and report formats hold on any codebase.

## Repository layout

```
.claude-plugin/marketplace.json     marketplace entry
plugins/lean-workflow/
  .claude-plugin/plugin.json        plugin manifest
  skills/                           feature, fix, debug, secure, review, ship
  agents/                           the ten lean-* agents
```

## Contributing

Issues and pull requests are welcome. One rule for new agents: if it cannot report its result in under a dozen lines, it does not belong here.

## License

[MIT](LICENSE) — free to use, modify and redistribute, for any purpose, commercial included.
