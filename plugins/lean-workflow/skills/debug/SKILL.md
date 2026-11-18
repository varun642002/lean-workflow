---
name: debug
description: Use automatically when the user asks why something is wrong, broken or slow without asking for a fix yet. Picks the right lean debugging agent.
argument-hint: <symptom, error text or slow screen>
---
Investigate: $ARGUMENTS

Pick one agent from the symptom and run it:
- Screen, layout, button, React state, console error → `lean-ui-debugger`
- Wrong numbers, data not saving, API/server error, stack trace → `lean-debugger`
- Slow or expensive → `lean-perf`
If the symptom spans front end and back end, run `lean-ui-debugger` and `lean-debugger` in parallel.

Show the root cause and proposed fix in ≤8 lines, then ask whether to apply it (use /fix flow on yes).
