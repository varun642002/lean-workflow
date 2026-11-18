# lean-workflow

Token-lean agents and workflow skills for [Claude Code](https://claude.com/claude-code).

Every agent here is built around the same three rules: **grep before you read**, **read by line range, never a whole large file**, and **report in a fixed, capped format**. The result is a full plan → build → verify → review loop that costs a fraction of the context an unconstrained agent spends on the same task.

## Install

```
/plugin marketplace add varun642002/lean-workflow
```

```
/plugin install lean-workflow@lean-workflow
```

## Skills

Each skill orchestrates the agents below, so you type intent and nothing else.

| Command | What it does |
|---|---|
| `/feature <what to build>` | Plans with `lean-planner`, implements (parallel `lean-builder`s for independent parts), then tests and reviews. |
| `/fix <symptom>` | Root cause via `lean-debugger`, minimal fix, verification. |
| `/debug <symptom>` | Investigation only — routes to the UI, logic or performance debugger and stops before changing anything. |
| `/secure [area \| "test"]` | `lean-security` audit; add `test` to also probe the running localhost app with `lean-security-tester`. |
| `/review [commit \| branch]` | `lean-reviewer` + `lean-tester` in parallel, bug-only findings. |
| `/ship [pr]` | Type check, branch if needed, commit in the repo's own message style, optionally open a PR. |

## Agents

| Agent | Model | Writes? | Purpose |
|---|---|---|---|
| `lean-planner` | inherit | no | Numbered plan with exact files and functions before any 2+ file change. |
| `lean-builder` | sonnet | yes | Implements one well-defined step; run several in parallel, one per file group. |
| `lean-debugger` | inherit | no | Back-end and logic bugs — wrong totals, failed saves, API errors, stack traces. |
| `lean-ui-debugger` | inherit | no | React/UI bugs; traces component → state → data → styling. |
| `lean-perf` | sonnet | no | Measured bottlenecks: DB queries in loops, re-renders, sequential awaits, bundle size. |
| `lean-security` | sonnet | no | Auth, authorization, security rules, injection, secrets, money/stock tampering. |
| `lean-security-tester` | inherit | no | Actively probes the app on localhost only; non-destructive, never prints secrets. |
| `lean-reviewer` | sonnet | no | Bug-only diff review, max 8 findings, no style nits and no praise. |
| `lean-tester` | haiku | no | Type check plus tests, failures only. The cheapest verification step. |
| `lean-docs` | sonnet | docs only | README, setup steps, API reference and env vars — restricted to `*.md`, `docs/**`, `.env.example` and doc comments. |

Only `lean-builder` may change source code; `lean-docs` may change documentation. Everything else is read-only by construction.

## Optional: automatic routing

To have Claude pick the right agent from a plain sentence, without you typing a slash command, add a table like this to your `CLAUDE.md`:

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

## Notes

The security, performance and UI agents mention Firestore and React because that is what they were written against. They work on any stack — the checklists simply skip what does not apply.

## License

MIT
