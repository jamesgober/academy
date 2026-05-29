<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 03](./README.md)

---

# Rust CI Workflows With GitHub Actions

> CI is a second pair of hands that runs the boring checks every time. It should
> catch formatting mistakes, lint warnings, broken tests, and feature-flag
> surprises before code lands on `main`.

**You will learn:**
- What CI is and why Rust projects use it
- A practical GitHub Actions workflow for Rust
- What each command checks
- How to read CI failures without panic
- How branch protection turns CI from a suggestion into a gate

**Before this page, you should know:** [Advanced Testing Options](./03-advanced-testing-options.md)

---

## What CI Means

CI stands for continuous integration.

In plain language:

> Every time code is pushed, another computer checks whether the project still
> builds, formats, lints, and passes tests.

Local checks are still important. CI protects the shared project from:

- "It worked on my machine"
- Forgotten tests
- Formatting drift
- Warnings that only happen on a clean environment
- Pull requests that accidentally break another feature

CI does not prove the software is perfect. It proves the checks you wrote still
pass in a repeatable environment.

---

## Local Commands First

Before CI exists, you should know the commands it will run.

```bash
cargo fmt --all -- --check
cargo clippy --all-targets --all-features -- -D warnings
cargo test --all-features
cargo test --doc
```

What they mean:

| Command | Purpose |
|---|---|
| `cargo fmt --all -- --check` | Verifies code is formatted without changing files |
| `cargo clippy --all-targets --all-features -- -D warnings` | Runs deeper lint checks and treats warnings as failures |
| `cargo test --all-features` | Runs tests with all feature flags enabled |
| `cargo test --doc` | Runs examples in documentation comments |

The `--` in `cargo fmt --all -- --check` separates Cargo arguments from
`rustfmt` arguments.

The `--` in `cargo clippy ... -- -D warnings` separates Cargo arguments from
Clippy/rustc lint arguments.

New coders often see double dashes and think something magical is happening. It
is just argument routing.

---

## Basic CI Workflow

Create this file:

```text
.github/
  workflows/
    rust.yml
```

`.github/workflows/rust.yml`:

```yaml
name: Rust CI

on:
  pull_request:
  push:
    branches: [main]

jobs:
  checks:
    name: Format, lint, and test
    runs-on: ubuntu-latest

    steps:
      - name: Check out repository
        uses: actions/checkout@v4

      - name: Install stable Rust
        uses: dtolnay/rust-toolchain@stable
        with:
          components: rustfmt, clippy

      - name: Cache Rust build output
        uses: Swatinem/rust-cache@v2

      - name: Check formatting
        run: cargo fmt --all -- --check

      - name: Run Clippy
        run: cargo clippy --all-targets --all-features -- -D warnings

      - name: Run tests
        run: cargo test --all-features

      - name: Run doc tests
        run: cargo test --doc
```

This workflow runs on:

- Every pull request
- Every push to `main`

That is a strong default for a beginner project.

---

## What Each Step Does

### Check Out Repository

```yaml
- name: Check out repository
  uses: actions/checkout@v4
```

GitHub Actions starts with an empty runner. This step downloads your repository
into that runner.

Without it, there is no code to test.

### Install Stable Rust

```yaml
- name: Install stable Rust
  uses: dtolnay/rust-toolchain@stable
  with:
    components: rustfmt, clippy
```

This installs the stable Rust toolchain and the extra tools used by the
workflow:

- `rustfmt` for formatting
- `clippy` for linting

If your project has a `rust-toolchain.toml`, the workflow should respect the
toolchain you pin there.

Example:

```toml
[toolchain]
channel = "stable"
components = ["rustfmt", "clippy"]
```

### Cache Rust Build Output

```yaml
- name: Cache Rust build output
  uses: Swatinem/rust-cache@v2
```

Rust builds can take time. A cache saves downloaded crates and build output so
future CI runs are faster.

If the cache has trouble, remove this step temporarily. Caching should speed up
CI, not make it mysterious.

### Check Formatting

```yaml
- name: Check formatting
  run: cargo fmt --all -- --check
```

This fails if code is not formatted.

Fix locally:

```bash
cargo fmt --all
```

Then commit the formatted files.

### Run Clippy

```yaml
- name: Run Clippy
  run: cargo clippy --all-targets --all-features -- -D warnings
```

Clippy catches common mistakes and style problems.

Examples:

- Needlessly cloning values
- Writing manual code that has a safer standard helper
- Suspicious comparisons
- Unused results
- Confusing control flow

`-D warnings` means warnings become errors. This keeps the project clean.

### Run Tests

```yaml
- name: Run tests
  run: cargo test --all-features
```

This runs unit and integration tests.

`--all-features` enables every Cargo feature. That matters when a project has
optional functionality.

### Run Doc Tests

```yaml
- name: Run doc tests
  run: cargo test --doc
```

This checks examples in documentation comments. Public docs should compile.

---

## Workspace Version

If your repository has multiple crates, the root `Cargo.toml` may define a
workspace:

```toml
[workspace]
members = [
    "crates/study-core",
    "crates/study-cli",
]
resolver = "2"
```

Use workspace-aware commands:

```yaml
- name: Check formatting
  run: cargo fmt --all -- --check

- name: Run Clippy
  run: cargo clippy --workspace --all-targets --all-features -- -D warnings

- name: Run tests
  run: cargo test --workspace --all-features

- name: Run doc tests
  run: cargo test --workspace --doc
```

Use `--workspace` when you want CI to check every crate in the workspace.

---

## Feature Flag Matrix

Some projects have optional features:

```toml
[features]
default = ["json"]
json = ["serde", "serde_json"]
csv = []
```

A hidden risk:

> Tests pass with all features enabled, but fail with default features only.

For a more serious library, test multiple feature sets:

```yaml
strategy:
  matrix:
    features:
      - ""
      - "--all-features"
      - "--no-default-features"

steps:
  - name: Check out repository
    uses: actions/checkout@v4

  - name: Install stable Rust
    uses: dtolnay/rust-toolchain@stable
    with:
      components: rustfmt, clippy

  - name: Run tests
    run: cargo test ${{ matrix.features }}
```

For beginners, start with one workflow. Add a matrix when feature flags become a
real part of the project.

---

## Reading A CI Failure

A red CI check is not a moral event. It is a report.

Use this flow:

```text
1. Open the failed job.
2. Find the first failed step.
3. Read the command that failed.
4. Read the first real error message.
5. Re-run that command locally.
6. Fix locally.
7. Push again.
```

Example:

```text
Run cargo fmt --all -- --check
Diff in /home/runner/work/app/src/lib.rs at line 12:
```

Meaning:

```text
CI failed because formatting is different from rustfmt's expected output.
Run cargo fmt --all locally, commit the change, push again.
```

Example:

```text
error: this `if` has identical blocks
  --> src/lib.rs:42:5
```

Meaning:

```text
Clippy found suspicious logic. Open src/lib.rs near line 42 and inspect the if.
```

Example:

```text
failures:
    rejects_empty_topic
```

Meaning:

```text
A test failed. Run cargo test rejects_empty_topic -- --nocapture locally.
```

---

## Branch Protection

CI is most useful when the project requires it before merging.

On GitHub, branch protection can require:

- Pull requests before merging
- Passing status checks
- Review approval
- No direct pushes to `main`

Beginner mental model:

```text
Branch protection is the lock.
CI is one of the keys.
```

Without branch protection, CI is advice. With branch protection, CI becomes a
quality gate.

---

## Local Script For Humans

CI commands should be easy to run locally. You can document them in `README.md`,
or create a small script.

Unix-style script:

```bash
#!/usr/bin/env bash
set -euo pipefail

cargo fmt --all -- --check
cargo clippy --all-targets --all-features -- -D warnings
cargo test --all-features
cargo test --doc
```

PowerShell version:

```powershell
$ErrorActionPreference = "Stop"

cargo fmt --all -- --check
cargo clippy --all-targets --all-features -- -D warnings
cargo test --all-features
cargo test --doc
```

The goal is simple:

> A contributor should be able to run the same checks before pushing.

---

## Common CI Mistakes

| Mistake | Symptom | Fix |
|---|---|---|
| Workflow file in wrong folder | Workflow never appears | Put it in `.github/workflows/*.yml` |
| Forgot checkout step | Cargo cannot find `Cargo.toml` | Add `actions/checkout` |
| Missing Clippy component | `cargo clippy` fails | Install `clippy` component |
| Formatting check fails | CI shows diff | Run `cargo fmt --all` locally |
| Test uses local-only file path | CI cannot find file | Use repo-relative fixtures |
| Test depends on time zone | Passes locally, fails in CI | Make time deterministic |
| Secrets used in pull request | Secret is unavailable | Mock external service or skip safely |

---

## Mini Project: Add CI To Study Tools

Create this project shape:

```text
study-tools/
  Cargo.toml
  src/
    lib.rs
  .github/
    workflows/
      rust.yml
```

Add one simple test:

```rust
pub fn total_minutes(minutes: &[u32]) -> u32 {
    minutes.iter().sum()
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn totals_empty_list_as_zero() {
        assert_eq!(total_minutes(&[]), 0);
    }
}
```

Run locally:

```bash
cargo fmt --all
cargo clippy --all-targets --all-features -- -D warnings
cargo test --all-features
```

Then push the branch and confirm GitHub Actions runs.

---

## Chapter Checkpoint

You should now be able to answer:

- What is CI?
- Why does CI need `actions/checkout`?
- What does `cargo fmt --all -- --check` verify?
- Why does Clippy use `-D warnings` in CI?
- Why might a library test multiple feature sets?
- What is the first thing to inspect when CI fails?
- Why does branch protection matter?

---

## Recap

- CI runs project checks in a clean environment.
- Rust CI should usually run formatting, Clippy, tests, and doc tests.
- A workflow file belongs in `.github/workflows/`.
- Make CI failures boring by running the same commands locally.
- Branch protection turns CI into a real quality gate.

## Try It Yourself

Add the baseline workflow to one Rust project. Then intentionally break
formatting, push a branch, and watch CI fail. Fix it with `cargo fmt --all`,
push again, and verify that CI turns green.

---

[**Next ->** Debugging Test Failures and Flaky Behavior](./05-debugging-test-failures-and-flaky-behavior.md)  
[**<- Previous** Advanced Testing Options](./03-advanced-testing-options.md)
