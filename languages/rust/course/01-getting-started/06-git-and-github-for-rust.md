<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 01](./README.md)

---

# Git And GitHub For Rust Projects

> A Rust project becomes much easier to trust when the repository has clean
> commits, a useful README, ignored build output, and repeatable validation.

**You will learn:**
- What Rust files should be committed
- What Rust files should be ignored
- How to write a useful first README
- How to make focused commits
- What to include in a pull request
- How local Cargo checks connect to CI later

**Before this page, you should know:**
- [Cargo Workspaces And Monorepos](./05-workspaces-and-monorepos.md)
- [Git and GitHub Basics](../../../../getting-started/git-and-github-basics.md)

---

## What To Commit

Commit source and project metadata:

```text
Cargo.toml
Cargo.lock
src/
tests/
examples/
README.md
.gitignore
rust-toolchain.toml, if the project pins a toolchain
.github/workflows/, if the project has CI
```

For course projects and applications, commit `Cargo.lock`.

Why?

```text
Cargo.toml says what versions are allowed.
Cargo.lock records the exact versions used.
```

That makes builds more repeatable.

---

## What Not To Commit

Do not commit generated build output:

```text
target/
```

Good `.gitignore`:

```gitignore
/target
```

Sometimes also ignore local editor or OS files:

```gitignore
.DS_Store
Thumbs.db
```

Be careful with broad ignore patterns. Do not ignore real source files by
accident.

---

## First Repository Setup

From your project root:

```bash
git init
git status
git add .
git status
git commit -m "chore: initialize rust project"
```

Use `git status` before and after staging. It teaches you what Git sees.

Expected first commit contents:

```text
.gitignore
Cargo.lock
Cargo.toml
README.md
src/main.rs or src/lib.rs
```

If `target/` appears in `git status`, stop and fix `.gitignore` before
committing.

---

## Write A Useful README

A beginner-friendly Rust README should answer:

```text
What is this project?
What does it require?
How do I run it?
How do I test it?
What commands should pass before a PR?
```

Template:

```markdown
# Study Tools

Small Rust exercises for tracking study sessions.

## Requirements

- Rust stable
- Cargo

## Run

```bash
cargo run
```

## Test

```bash
cargo fmt --all -- --check
cargo check
cargo test
cargo clippy --all-targets --all-features -- -D warnings
```
```

Keep the README honest. If commands change, update it.

---

## Commit Style

Good commits are small and named after intent.

Examples:

```text
chore: initialize rust project
feat: add study entry parser
test: cover invalid minutes input
docs: add setup commands to readme
fix: reject empty topic names
refactor: split parser module
```

Weak commits:

```text
stuff
updates
fix
wip
final final
```

Your future self will read this history. Be kind to that person.

---

## Branch Workflow

Starter workflow:

```bash
git switch -c feat/study-entry-parser
```

Do work.

Run checks:

```bash
cargo fmt --all -- --check
cargo check
cargo test
cargo clippy --all-targets --all-features -- -D warnings
```

Commit:

```bash
git add .
git commit -m "feat: add study entry parser"
```

Push:

```bash
git push -u origin feat/study-entry-parser
```

Open a pull request on GitHub.

---

## Pull Request Checklist

A useful PR description includes:

```text
What changed:
Why it changed:
How I validated it:
Risk or tradeoff:
```

Example:

```markdown
## What Changed

- Added `StudyEntry`
- Added `parse_entry`
- Added edge-case tests for empty topic and invalid minutes

## Why

The CLI needs reusable parsing before it can read study logs from files.

## Validation

- `cargo fmt --all -- --check`
- `cargo check`
- `cargo test`
- `cargo clippy --all-targets --all-features -- -D warnings`
```

Do not make reviewers guess what happened.

---

## Avoid Risky Git Habits

Avoid:

- Force-pushing shared branches without team agreement
- Mixing huge refactors with feature changes
- Committing `target/`
- Committing secrets or API keys
- Committing broken code to `main`
- Ignoring failing tests because "it works locally"

If you accidentally commit something sensitive, do not just delete it in the next
commit. Ask for help. Secrets in Git history need proper cleanup and rotation.

---

## Rust-Specific Repository Files

Optional but useful:

`rust-toolchain.toml`:

```toml
[toolchain]
channel = "stable"
components = ["rustfmt", "clippy"]
```

Use this when a project wants everyone on the same toolchain channel and
components.

`.cargo/config.toml`:

```toml
[alias]
q = "check"
t = "test"
```

Use Cargo config carefully. It can be useful, but beginners should understand
the normal commands before adding aliases.

---

## How This Leads To CI

Local quality gate:

```text
You run cargo fmt/check/test/clippy before pushing.
```

CI quality gate:

```text
GitHub Actions runs the same checks on every PR.
```

You will build CI in Chapter 03. For now, build the habit locally.

---

## Mini Project: Make A Collaboration-Ready Repo

Take your `hello-rust` or `study-tools` project and add:

- `.gitignore` with `/target`
- `README.md` with run/test commands
- One clean first commit
- One feature branch
- One second commit that changes code

Run:

```bash
git log --oneline --decorate --graph --all
```

You should see a readable history.

---

## Chapter Checkpoint

You should now be able to answer:

- Why should `target/` be ignored?
- Why do applications commit `Cargo.lock`?
- What belongs in a Rust README?
- What makes a good commit message?
- What should a PR description include?
- Why should local Cargo checks run before pushing?
- Why are secrets in Git history serious?

---

## Recap

- Commit source, manifests, lockfiles, tests, examples, docs, and CI config.
- Do not commit `target/`.
- Keep README commands current.
- Use focused branches and focused commits.
- Validate with Cargo before opening pull requests.
- CI later automates the same checks.

## Try It Yourself

Create a branch named `docs/add-readme-commands`, update your README with the
quality gate commands, run the commands, commit the README, and inspect your
history with `git log --oneline`.

---

[**Next ->** Chapter 02: Rust Core Mental Model](../02-rust-core-mental-model/README.md)  
[**<- Previous** Cargo Workspaces And Monorepos](./05-workspaces-and-monorepos.md)
