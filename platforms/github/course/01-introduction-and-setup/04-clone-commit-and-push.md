<h1 align="center">
    <img width="99" alt="GitHub logo" src="../../../../_assets/logos/github.svg">
    <br>
    <b>GitHub</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [GitHub](../../README.md) · [Chapter 01](./README.md)

</div>

---

# Clone, Commit, and Push

> This is the core loop: copy project locally, make changes, save history, and send updates to GitHub.

**You will learn:**
- How to clone a repository
- How to create a clean commit
- How to push changes safely

**Before this page, you should know:**
- [Create Your First Repository](./03-create-your-first-repository.md)

---

## Clone repository

Use your repository URL:

```bash
git clone https://github.com/<username>/<repo>.git
cd <repo>
```

<!-- SCREENSHOT: Repository Code button showing HTTPS clone URL -->

## Make and commit a change

Create or edit `README.md`, then run:

```bash
git status
git add .
git commit -m "docs: update readme intro"
```

## Push to GitHub

```bash
git push origin main
```

<!-- SCREENSHOT: Commit and push reflected in GitHub repository commit history -->

## Understand the flow

- `clone`: copy remote repo to your machine.
- `commit`: save a snapshot of your changes locally.
- `push`: send local commits to GitHub.

> [!IMPORTANT]
> Commit messages should explain intent. Future you will rely on this history.

> [!WARNING]
> Avoid `git add .` in large repos unless you verify what changed with `git status` first.

## Related reference

- [GitHub Quick Reference](../../reference/github-quick-reference.md)
- [GitHub Troubleshooting and Recovery](../../reference/github-troubleshooting-and-recovery.md)

---

## Recap

- You cloned a repository and changed a file.
- You committed with a clear message.
- You pushed history to GitHub.

## Try it yourself

Make one additional README change and push a second commit with a distinct,
intent-focused commit message.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Create Your First Repository](./03-create-your-first-repository.md) | [Chapter 01](./README.md) · [GitHub](../../README.md) · [Home](../../../../README.md) | [Chapter 02 — Repositories and Structure →](../02-repositories-and-structure/README.md) |

</div>
