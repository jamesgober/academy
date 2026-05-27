<h1 align="center">
    <img width="99" alt="GitHub logo" src="../../../../_assets/logos/github.svg">
    <br>
    <b>GitHub</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [GitHub](../../README.md) · [Chapter 03](./README.md)

</div>

---

# Branches: What, When, and Why

> A branch is an isolated line of work so you can change code without breaking the main line.

**You will learn:**
- What a branch is in plain language
- When to create a branch
- Why teams avoid direct pushes to main

**Before this page, you should know:** [Repository Settings That Matter](../02-repositories-and-structure/05-repository-settings-that-matter.md)

---

## What a branch is

Think of `main` as the stable timeline and branches as safe side roads.
You build on a branch, then merge when ready.

## When to branch

Create a branch for:
- new feature
- bug fix
- documentation update
- refactor

## Why this matters

Branching provides:
- safer experimentation
- cleaner pull requests
- easier review and rollback

> [!IMPORTANT]
> Never treat `main` as a scratchpad in shared repositories.

## Naming branch conventions

Use prefix + scope + intent:

```text
feature/add-github-pages-guide
fix/release-tag-format
docs/update-contributing-section
```

---

## Recap

- Branches isolate work.
- Shared repos should protect `main`.
- Good branch names improve team communication.

## Try it yourself

Create a branch named `docs/add-changelog-section` in a test repository.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Chapter Start](./README.md) | [Chapter 03](./README.md) · [GitHub](../../README.md) · [Home](../../../../README.md) | [Working on Branches Day to Day →](./02-working-on-branches-day-to-day.md) |

</div>
