# 2026-05-27 C# NOTES Deepening Pass

## Summary
Upgraded C# content from concise-first draft to a deeper NOTES-aligned learning pass, focusing on practical decision-making, alternatives, and real debugging workflow.

## Deepened course pages
- `course/01-getting-started/01-installing-the-dotnet-sdk.md`
  - Added OS-specific install flow guidance, real project verification loop, and common failure triage.
- `course/02-core-language-basics/02-methods-parameters-and-returns.md`
  - Added `in` and `params` examples, overload vs optional parameter guidance, return-style heuristics, and signature checklist.
- `course/02-core-language-basics/03-conditionals-switch-and-pattern-matching.md`
  - Added guard-clause/no-else patterns, switch statement variant, nested conditional guidance, and common control-flow mistakes.
- `course/04-collections-exceptions-and-data/02-exception-handling-and-failure-design.md`
  - Added catch-boundary strategy, standard exception type guidance, rethrow stack-trace guardrails, and failure-design checklist.
- `course/05-async-testing-and-capstone/04-capstone-console-order-tracker.md`
  - Added architecture layering, domain sketch, milestones, expected outputs, and baseline test plan.

## Deepened reference pages
- `reference/commands-and-build-flags.md`
  - Added restore/format flow, warnings-as-errors usage, and recommended local quality loop.
- `reference/methods-and-parameters-cheat-sheet.md`
  - Added named/default args, tuple returns, and common anti-patterns.
- `reference/conditionals-and-loops-patterns.md`
  - Added guard clauses, switch statement variant, pattern matching variant, and loop control notes.
- `reference/errors-warnings-and-debugging-guide.md`
  - Added additional common compiler IDs, warning triage policy, first-pass compiler triage, and repro checklist.

## Validation
- `get_errors` reported no errors under `languages/csharp`.
- Lesson navigation chrome marker check reported `NAV_MARKER_ISSUES=0`.
- Link integrity scan reported `BROKEN_LINK_COUNT=0`.
