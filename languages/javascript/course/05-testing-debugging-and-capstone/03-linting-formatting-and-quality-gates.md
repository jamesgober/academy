<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [JavaScript](../../README.md) · [Chapter 05](./README.md)

</div>

---

# Linting, Formatting, and Quality Gates

> Quality tooling catches defects and inconsistency before code review.

**You will learn:**
- what linting is and why it matters
- formatter role in team consistency
- practical local quality-gate loop

**Before this page, you should know:** test runner basics.

---

## Linting and formatting roles

- linting: rule-based code correctness/style checks
- formatting: automatic code layout normalization

Typical tooling:
- ESLint
- Prettier

## Suggested loop

1. run lint checks
2. run test checks
3. run formatting checks

Use the command style your chosen environment provides.

## Why this matters

- catches unused variables and unsafe patterns
- reduces style arguments in reviews
- enforces repeatable baseline for all contributors

---

## Recap

- Linting and testing should run before commits.
- Formatting is automation, not personal preference.
- Quality gates reduce regressions and drift.

## Try it yourself

Create one lint rule violation, run lint, then fix and rerun.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Debugging Workflow and Tooling](./02-debugging-workflow-and-tooling.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Capstone: Task Tracker CLI →](./04-capstone-task-tracker-cli.md) |

</div>
