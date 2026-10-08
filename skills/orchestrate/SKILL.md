---
name: orchestrate
description: Run the full orchestration loop on a feature or change - understand, design, decompose into tasks, delegate code to the implementer agent (latest Opus) and research/tests to the researcher and tester agents (latest Sonnet), then verify and integrate. Use for any multi-file or design-heavy piece of work.
---

Apply the orchestration protocol from `.claude/rules/orchestration.md` to this request:

$ARGUMENTS

Start with triage. If the request is trivial, say so and do it directly. Otherwise work through the loop: research in parallel where questions are independent, write the design and acceptance criteria before delegating, send one self-contained brief per task, review each diff yourself, verify with the tester, and finish with a report that separates what was verified from what was not.
