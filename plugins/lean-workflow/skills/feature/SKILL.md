---
name: feature
description: Use automatically whenever the user asks to build, add or change a feature or screen. Runs plan, build, check, review with the lean agents.
argument-hint: <what to build>
---
Build this feature: $ARGUMENTS

1. Small (1–2 files, obvious): skip planning, implement directly.
   Otherwise: run `lean-planner` with the request; show its plan in ≤10 lines.
2. Implement. Do steps yourself when they depend on each other; send independent steps to `lean-builder` agents in parallel (one per file group, never two on the same file).
3. Run `lean-tester`. Fix failures.
4. If the change is visible in the app, check it in the browser preview (text tools first, one screenshot at the end).
5. Run `lean-reviewer` on the diff. Fix real findings.
6. Report in ≤6 lines: what changed, what was verified, what was not. Do not commit unless asked.

Token rules: grep before reading, read by line range, never re-read a file you just edited, trim command output.
