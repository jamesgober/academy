<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 01](./README.md)

</div>

---

# Git and GitHub for Rust Projects

> Prepare your Rust project for real collaboration before moving into language features.

**You will learn:**
- A clean Git workflow for Rust projects
- What to include in the first repository commit
- How to avoid common collaboration mistakes

**Before this page, you should know:**
- [Cargo Workspaces and Monorepos](./05-workspaces-and-monorepos.md)
- [Git and GitHub Basics](../../../../getting-started/git-and-github-basics.md)

---

## First repository checklist

From your Rust project root:

```bash
git init
git add .
git commit -m "chore: initialize rust project"
```

Make sure these files exist before pushing:
- `Cargo.toml`
- `src/`
- `.gitignore`
- `README.md`

## Recommended `.gitignore` entries

Cargo projects should ignore build output:

```text
/target
```

> [!IMPORTANT]
> Do not commit `target/`. Build artifacts are machine-generated and large.

## Branch strategy (starter)

- `main` stays stable.
- Create feature branches for changes.
- Open pull requests for review.

## What to include in pull requests

- What changed
- Why it changed
- How it was validated (`cargo check`, `cargo test`, `cargo run`)

> [!TIP]
> Add command output snippets in PR descriptions when setup or build behavior changes.

## Risky patterns to avoid

> [!WARNING]
> Avoid force-pushing shared branches unless your team has an explicit policy for it.

> [!WARNING]
> Avoid combining refactors and feature work in one PR. It makes review harder and regressions easier.

---

## Recap

- Rust project collaboration starts with clean Git history and sensible defaults.
- Keep `main` stable and use focused feature branches.
- Validate with Cargo commands before opening pull requests.

## Try it yourself

Create a branch named `chore/add-readme`, improve the README, commit, and push.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Cargo Workspaces and Monorepos](./05-workspaces-and-monorepos.md) | [Chapter 01](./README.md) · [Rust](../../README.md) · [Home](../../../../README.md) | [Rust Core Mental Model (Chapter 02) →](../02-rust-core-mental-model/README.md) |

</div>
