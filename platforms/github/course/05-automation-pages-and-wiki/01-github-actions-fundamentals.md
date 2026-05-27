<h1 align="center">
    <img width="99" alt="GitHub logo" src="../../../../_assets/logos/github.svg">
    <br>
    <b>GitHub</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [GitHub](../../README.md) · [Chapter 05](./README.md)

</div>

---

# GitHub Actions Fundamentals

> GitHub Actions automates repeated tasks like tests, linting, and releases.

**You will learn:**
- What workflows, jobs, and steps are
- Why CI matters for project quality
- A safe beginner-first workflow pattern

**Before this page, you should know:** [Attracting and Retaining Contributors](../04-community-and-visibility/04-attracting-and-retaining-contributors.md)

---

## Core concepts

- **Workflow**: automation file in `.github/workflows/`.
- **Job**: group of steps running in one environment.
- **Step**: single command or action.

## Beginner CI workflow goals

- run tests on pull requests
- run lint checks
- block merge on failed checks

> [!IMPORTANT]
> Keep first workflows simple. Complexity can be added after stable basics.

---

## Recap

- Actions automate project quality checks.
- CI catches problems before merge.
- Start small, then expand safely.

## Try it yourself

Create a workflow that runs on pull requests and prints a simple confirmation log.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Chapter Start](./README.md) | [Chapter 05](./README.md) · [GitHub](../../README.md) · [Home](../../../../README.md) | [Setting Up a Basic CI Workflow →](./02-setting-up-a-basic-ci-workflow.md) |

</div>
