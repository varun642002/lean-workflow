---
name: secure
description: Use automatically for any security request - audit, check, harden, pentest or verify a security fix. Runs lean-security (and lean-security-tester on localhost when testing is asked).
argument-hint: [area, commit or "test"; blank = current changes]
---
Security check. Argument: $ARGUMENTS (blank = uncommitted changes; an area like "auth" or "billing API" = that area; "test" = also probe the running app)

1. Run `lean-security` on the target.
2. If the argument contains "test": make sure the dev server is running on localhost (start it with the project's preview config if needed), then run `lean-security-tester` in parallel with step 1.
3. Merge results, drop duplicates, sort CRITICAL → MEDIUM. Show at most 10 lines.
4. Ask whether to fix them; on yes, fix CRITICAL/HIGH first, then rerun `lean-security` on the diff.
