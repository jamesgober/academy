<h1 align="center">
    <img width="99" alt="GitHub logo" src="../../../../_assets/logos/github.svg">
    <br>
    <b>GitHub</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [GitHub](../../README.md) · [Chapter 03](./README.md)

</div>

---

# Merging and Conflict Handling

> Merging combines branch work into main. Conflicts happen when changes overlap.

**You will learn:**
- Merge methods and when to use each
- How to resolve merge conflicts safely
- Risky recovery commands and warnings

**Before this page, you should know:** [Issues and Pull Requests Workflow](./03-issues-and-pull-requests-workflow.md)

---

## Merge methods

- **Merge commit**: preserves full branch history.
- **Squash merge**: combines branch commits into one clean commit.
- **Rebase merge**: linear history, but can be harder for beginners.

> [!TIP]
> For beginner teams, squash merges are usually easiest to maintain.

## Conflict resolution flow

```bash
git checkout main
git pull
git checkout feature/my-change
git merge main
# resolve conflicts in files
git add .
git commit
```

Then push and update your pull request.

## About force push

Sometimes history rewrite requires force push:

```bash
git push --force-with-lease
```

> [!WARNING]
> `--force-with-lease` can overwrite remote history. Use only on your own branch
> and never on shared protected branches.

---

## Recap

- Choose merge style intentionally.
- Conflicts are normal and solvable.
- Force push is advanced and risky; use it carefully.

## Try it yourself

Create a conflict in a test repo, resolve it, and merge successfully.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Issues and Pull Requests Workflow](./03-issues-and-pull-requests-workflow.md) | [Chapter 03](./README.md) · [GitHub](../../README.md) · [Home](../../../../README.md) | [Chapter 04 — Community and Visibility →](../04-community-and-visibility/README.md) |

</div>
