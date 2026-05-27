<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 01](./README.md)

</div>

---

# Rust Project Structure

> Learn the standard project layout so every Cargo project feels familiar.

**You will learn:**
- The role of `Cargo.toml`, `src/`, `tests/`, and `examples/`
- A beginner-safe project structure that scales
- Which folders should not be committed

**Before this page, you should know:**
- [Create Your First Cargo Project](./02-first-cargo-project.md)

---

## Standard layout

```text
my-project/
├── Cargo.toml
├── src/
│   ├── main.rs
│   └── lib.rs   (optional)
├── tests/       (optional integration tests)
├── examples/    (optional runnable examples)
└── .gitignore
```

## What each part is for

- `src/main.rs`: executable entry point for binary projects.
- `src/lib.rs`: library root for reusable code.
- `tests/`: black-box tests that use your public API.
- `examples/`: sample programs users can run.

> [!TIP]
> If your project starts as an app and grows reusable pieces, add `src/lib.rs`
> and move shared logic there.

## What not to commit

Build output belongs in `target/` and should stay ignored by Git.

> [!WARNING]
> Committing `target/` bloats repositories and creates noisy diffs. Keep it out of version control.

## Recommended starter structure for learners

For course projects, this is enough:

```text
my-project/
├── Cargo.toml
├── src/
│   └── main.rs
└── README.md
```

Add `tests/` and `examples/` when you have a clear need.

---

## Recap

- Rust projects follow a predictable structure.
- `src/` holds source code; `Cargo.toml` defines package metadata.
- Keep version control clean by ignoring build output.

## Try it yourself

Create an `examples/` folder with `hello.rs`, then run it using `cargo run --example hello`.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Create Your First Cargo Project](./02-first-cargo-project.md) | [Chapter 01](./README.md) · [Rust](../../README.md) · [Home](../../../../README.md) | [Cargo Workflow Essentials →](./04-cargo-workflow.md) |

</div>
