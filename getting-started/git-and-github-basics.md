<div align="center">

[Home](../README.md) · [Getting Started](./README.md)

</div>

---

# Git and GitHub Basics

> The minimum Git and GitHub workflow you need before collaborative language projects.

> [!IMPORTANT]
> Want the full beginner-to-advanced path? Start the
> [GitHub Masterclass](../platforms/github/).

**You will learn:**
- What Git and GitHub do (and do not do)
- A safe first workflow for commits and push
- Which habits prevent history and merge pain

---

## Git vs GitHub

- **Git** is version control software that runs locally.
- **GitHub** is a hosted platform for remote repositories and collaboration.

You can use Git without GitHub, but most team workflows use both.

## Recommended first workflow

1. Create a local project folder.
2. Run `git init`.
3. Add a `.gitignore` before your first commit.
4. Commit small, meaningful changes.
5. Push to GitHub after each stable checkpoint.

Example:

```bash
git init
git add .
git commit -m "chore: initialize rust project"
```

## Commit quality rules

- One idea per commit.
- Commit messages should explain intent, not just action.
- Avoid giant "misc changes" commits.

> [!TIP]
> A clean commit history is part of project documentation.

## Common risks

> [!WARNING]
> Never commit secrets such as API keys, tokens, or `.env` files.

> [!WARNING]
> Avoid force-pushing shared branches unless your team explicitly agrees to that workflow.

## Next

- Full track: [GitHub Masterclass](../platforms/github/)
- Full course index: [GitHub Course](../platforms/github/course/)
- Quick lookup pages: [GitHub Reference](../platforms/github/reference/)
- Rust chapter companion: [Git and GitHub for Rust Projects](../languages/rust/course/01-getting-started/06-git-and-github-for-rust.md)
- Continue setup: [Setting Up Your Dev Folder](./dev-folder-setup.md)

---

<div align="center">

[← Getting Started](./README.md) · [Home](../README.md)

</div>
