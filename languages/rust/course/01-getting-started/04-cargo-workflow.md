<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 01](./README.md)

---

# Cargo Workflow Essentials

> Cargo is not just a build command. It is the loop that keeps Rust work calm:
> format, check, test, run, document, repeat.

**You will learn:**
- The daily Cargo command loop
- Which commands are fast checks and which produce output
- How to read common Cargo output
- How to add and remove dependencies safely
- How to use Clippy and docs early
- How to build a beginner quality gate

**Before this page, you should know:** [Rust Project Structure](./03-rust-project-layout.md)

---

## The Daily Loop

Use this order while learning:

```bash
cargo fmt
cargo check
cargo test
cargo run
```

Why this order?

```text
format -> make code consistent
check  -> catch compile errors quickly
test   -> prove behavior
run    -> try the program as a user
```

This loop prevents the classic beginner spiral:

```text
write lots of code
run it
see 47 errors
feel doomed
```

Instead, you get feedback early.

---

## `cargo fmt`

Formats code using `rustfmt`:

```bash
cargo fmt
```

Before committing or CI:

```bash
cargo fmt --all -- --check
```

Plain meaning:

```text
Format all Rust code.
In check mode, report formatting problems without changing files.
```

Beginner rule:

> Do not argue with rustfmt. Let formatting become automatic.

---

## `cargo check`

Checks whether code compiles without building the final executable:

```bash
cargo check
```

Why it is useful:

- Faster than a full build
- Catches type errors
- Catches borrow checker errors
- Helps you iterate while writing

Example workflow:

```text
Write one function.
Run cargo check.
Fix compiler feedback.
Write the next function.
```

This is how Rust becomes less intimidating.

---

## `cargo build`

Builds the project:

```bash
cargo build
```

Debug build output:

```text
target/debug/<program-name>
```

Release build:

```bash
cargo build --release
```

Release build output:

```text
target/release/<program-name>
```

Debug builds compile faster and include better debugging information.

Release builds optimize for speed but take longer to compile.

Use:

```text
cargo build          while developing
cargo build --release when measuring or shipping
```

---

## `cargo run`

Builds and runs a binary:

```bash
cargo run
```

Pass arguments to your program after `--`:

```bash
cargo run -- Rust 45
```

Why `--`?

```text
Before --  -> arguments for Cargo
After --   -> arguments for your program
```

Example:

```bash
cargo run --bin summarize -- sessions.txt
```

Meaning:

```text
Run Cargo binary named summarize.
Pass sessions.txt to that binary.
```

---

## `cargo test`

Runs tests:

```bash
cargo test
```

Run one test by name:

```bash
cargo test rejects_empty_topic
```

Show printed output:

```bash
cargo test rejects_empty_topic -- --nocapture
```

Run doc tests:

```bash
cargo test --doc
```

You will learn testing deeply in Chapter 03. For now, know this:

> `cargo test` is how Rust projects prove behavior repeatedly.

---

## `cargo clippy`

Clippy is Rust's linter:

```bash
cargo clippy
```

Strict mode:

```bash
cargo clippy --all-targets --all-features -- -D warnings
```

Plain meaning:

```text
Check library, binary, tests, examples, and all features.
Treat warnings as errors.
```

Use strict Clippy before merging code. When you are brand new, plain
`cargo clippy` may feel friendlier.

---

## `cargo doc`

Generate documentation:

```bash
cargo doc --open
```

For dependencies too:

```bash
cargo doc --open --document-private-items
```

Use docs to inspect your own public API. If your generated docs are confusing,
your API may need clearer names or examples.

---

## Dependencies

Add a dependency:

```bash
cargo add rand
```

Remove a dependency:

```bash
cargo remove rand
```

Update dependencies according to `Cargo.toml` rules:

```bash
cargo update
```

Search online:

```text
crates.io -> package registry
docs.rs   -> generated API docs
```

Beginner dependency checklist:

```text
Do I need this crate?
Is it maintained?
Are docs clear?
Does it solve more than it complicates?
Can standard library solve this for now?
```

---

## Read Cargo Output

Common output:

```text
Compiling hello-rust v0.1.0
Finished `dev` profile [unoptimized + debuginfo]
Running `target/debug/hello-rust`
```

Plain meaning:

```text
Cargo compiled your project.
It used the development profile.
It ran the debug executable.
```

Common error:

```text
error: could not find `Cargo.toml`
```

Meaning:

```text
You are not inside a Cargo project folder.
```

Fix:

```bash
cd path/to/project
```

Common compile error shape:

```text
error[E0308]: mismatched types
 --> src/main.rs:2:18
```

Read:

```text
error code
short message
file and line
help notes
```

Rust compiler messages are part of the learning experience. Read them slowly.

---

## Profiles

Cargo has build profiles.

| Profile | Command | Use |
|---|---|---|
| dev | `cargo build`, `cargo run` | Fast local development |
| release | `cargo build --release` | Optimized builds |
| test | `cargo test` | Test builds |

You can configure profiles in `Cargo.toml`, but do not start there. Defaults are
good for learners.

---

## Beginner Quality Gate

Before you call a Rust exercise done, run:

```bash
cargo fmt --all -- --check
cargo check
cargo test
cargo clippy --all-targets --all-features -- -D warnings
```

If it is a runnable app:

```bash
cargo run
```

If it has public docs:

```bash
cargo test --doc
```

This is the same mindset professional Rust teams use in CI.

---

## Mini Project: Build A Quality Loop Script

Create a file named `quality.ps1` on Windows:

```powershell
$ErrorActionPreference = "Stop"

cargo fmt --all -- --check
cargo check
cargo test
cargo clippy --all-targets --all-features -- -D warnings
```

Or `quality.sh` on macOS/Linux:

```bash
#!/usr/bin/env bash
set -euo pipefail

cargo fmt --all -- --check
cargo check
cargo test
cargo clippy --all-targets --all-features -- -D warnings
```

You do not need scripts for tiny exercises, but writing one helps you see the
quality loop as a repeatable habit.

---

## Chapter Checkpoint

You should now be able to answer:

- What command formats Rust code?
- Why is `cargo check` useful?
- What is the difference between `cargo build` and `cargo run`?
- How do you pass arguments to your program through Cargo?
- What does Clippy do?
- What does `cargo doc --open` show?
- What should you run before committing a Rust change?

---

## Recap

- Use `cargo fmt`, `cargo check`, `cargo test`, and `cargo run` as your daily loop.
- Use `cargo clippy` to catch suspicious code.
- Use `cargo doc` to inspect API docs.
- Add dependencies intentionally.
- Read Cargo and compiler output slowly; it usually tells you where to look.

## Try It Yourself

In your current practice project:

1. Break formatting and run `cargo fmt`.
2. Create a type error and run `cargo check`.
3. Add one test and run `cargo test`.
4. Run `cargo clippy`.
5. Run the final program with `cargo run`.

---

[**Next ->** Cargo Workspaces And Monorepos](./05-workspaces-and-monorepos.md)  
[**<- Previous** Rust Project Structure](./03-rust-project-layout.md)
