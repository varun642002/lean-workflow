---
name: ship
description: Use automatically when the user asks to commit, push, ship, release or open a PR. Checks, then commits in the repo style.
argument-hint: [pr]
---
Ship the current changes. Argument: $ARGUMENTS

1. `git status --short` and `git diff --stat`. If on main/master, create a branch first.
2. Run `lean-tester`. Stop and report if it fails.
3. Commit with a message in the repo's own style (check `git log --format=%s -5`): one plain sentence saying what the user can now do. Stage only files belonging to this change; never stage CSVs, images, .env or generated folders unless asked.
4. If the argument is "pr": push and open a PR with a 3–5 line description.
5. Report in ≤4 lines: branch, commit, PR link if any.
