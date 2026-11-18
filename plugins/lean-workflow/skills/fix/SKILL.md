---
name: fix
description: Use automatically whenever the user reports a bug, error or wrong result and wants it fixed. Root cause, minimal fix, check.
argument-hint: <symptom, error or failing case>
---
Fix this bug: $ARGUMENTS

1. If the cause is not obvious within two searches, run `lean-debugger` with the symptom.
2. Apply the minimal fix at the root cause; no unrelated changes.
3. Run `lean-tester`. If the bug shows in the app, reproduce the case in the browser preview to confirm it is gone.
4. Report in ≤5 lines: cause (`file:line`), fix, how it was verified. Do not commit unless asked.

Token rules: grep before reading, read by line range, never re-read a file you just edited, trim command output.
