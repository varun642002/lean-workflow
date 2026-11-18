---
name: review
description: Use automatically when the user asks to review or check changes, a branch or a commit. Bug-only review plus type check.
argument-hint: [commit, branch or blank for working tree]
---
Review target: $ARGUMENTS (if blank: the uncommitted working tree)

Run `lean-reviewer` and `lean-tester` in parallel on that target. Show the reviewer's findings as given and the tester's result in one line. Then ask whether to fix the findings; do not fix without a yes.
