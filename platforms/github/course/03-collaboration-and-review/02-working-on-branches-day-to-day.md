<h1 align="center">
    <img width="99" alt="GitHub logo" src="../../../../_assets/logos/github.svg">
    <br>
    <b>GitHub</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [GitHub](../../README.md) · [Chapter 03](./README.md)

</div>

---

# Working on Branches Day to Day

> Branch workflow is a repeatable loop: create, change, commit, sync, push.

**You will learn:**
- The branch command flow
- How to keep your branch updated
- How to avoid common beginner mistakes

**Before this page, you should know:** [Branches: What, When, and Why](./01-branches-what-when-and-why.md)

---

## Standard branch loop

```bash
git checkout -b feature/my-change
# edit files
git add .
git commit -m "feat: add my change"
git push -u origin feature/my-change
```

## Keep branch current

```bash
git checkout main
git pull
git checkout feature/my-change
git merge main
```

> [!TIP]
> Sync from `main` before opening a pull request. It reduces merge conflicts.

## Avoid these mistakes

- Working too long without pushing.
- Mixing unrelated changes in one branch.
- Opening huge pull requests.

---

## Recap

- Branch work should be small, focused, and frequently synced.
- Frequent pushes protect your work and improve visibility.
- Clean branch discipline makes review easier.

## Try it yourself

Create a branch, make one docs change, commit, and push it to your remote.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Branches: What, When, and Why](./01-branches-what-when-and-why.md) | [Chapter 03](./README.md) · [GitHub](../../README.md) · [Home](../../../../README.md) | [Issues and Pull Requests Workflow →](./03-issues-and-pull-requests-workflow.md) |

</div>
