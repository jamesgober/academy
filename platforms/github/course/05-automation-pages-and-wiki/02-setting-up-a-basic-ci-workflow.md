<h1 align="center">
    <img width="99" alt="GitHub logo" src="../../../../_assets/logos/github.svg">
    <br>
    <b>GitHub</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [GitHub](../../README.md) · [Chapter 05](./README.md)

</div>

---

# Setting Up a Basic CI Workflow

> A basic CI workflow automatically runs checks on every pull request.

**You will learn:**
- How to create your first CI workflow file
- Which checks should run first in a beginner project
- How to connect CI checks to branch protection

**Before this page, you should know:** [GitHub Actions Fundamentals](./01-github-actions-fundamentals.md)

---

## What CI means in plain language

**CI** (continuous integration) means your repository automatically verifies
changes before merge.

For beginners, CI should answer:
- does it build?
- do tests pass?
- does linting/formatting pass?

## Create workflow file

Create this file:

- `.github/workflows/ci.yml`

Starter workflow:

```yaml
name: CI

on:
  pull_request:
  push:
    branches: [main]

jobs:
  checks:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Rust
        uses: dtolnay/rust-toolchain@stable

      - name: Cache cargo
        uses: Swatinem/rust-cache@v2

      - name: Format check
        run: cargo fmt --all -- --check

      - name: Lint check
        run: cargo clippy --all-targets --all-features -- -D warnings

      - name: Test
        run: cargo test --all-features
```

> [!IMPORTANT]
> Keep CI small at first. Add more jobs only when your team can maintain them.

## Connect CI to branch protection

In repository settings:

1. Open branch protection for `main`.
2. Require status checks to pass before merge.
3. Select your CI workflow check.

This prevents unverified changes from landing on main.

<!-- SCREENSHOT: Branch protection rule requiring CI status checks -->

## Language-specific testing note

This chapter gives platform-level CI basics.
Each language track should define its own testing matrix and commands.

For Rust, typical checks are:
- `cargo fmt --check`
- `cargo clippy ...`
- `cargo test`

---

## Recap

- CI automates build/test/lint validation.
- Start with one reliable workflow file.
- Enforce CI with branch protection.

## Try it yourself

Create `ci.yml`, open a pull request with an intentional formatting issue, and
verify CI fails. Then fix it and verify CI passes.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← GitHub Actions Fundamentals](./01-github-actions-fundamentals.md) | [Chapter 05](./README.md) · [GitHub](../../README.md) · [Home](../../../../README.md) | [GitHub Pages for Project Docs →](./03-github-pages-for-project-docs.md) |

</div>
