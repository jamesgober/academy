<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 01](./README.md)

</div>

---

# Create Your First Cargo Project

> Start with Cargo from day one so your project layout and build flow are standard.

**You will learn:**
- How to create a binary Rust project with Cargo
- What files Cargo generates and why they exist
- How to run the default project successfully

**Before this page, you should know:**
- [Installing Rust](./01-installing-rust.md)
- [Filesystem Navigation](../../../../getting-started/filesystem-navigation.md)

---

## Create the project

From your `learning` folder, create a new project:

```bash
cargo new hello-rust
cd hello-rust
```

This creates a **binary** project (an executable program) by default.

> [!TIP]
> Use lowercase, hyphen-separated project names. They stay readable in terminals,
> package names, and repository URLs.

## Run it

```bash
cargo run
```

Cargo compiles your code and runs the executable. On first run, compile time is
longer because dependencies and build artifacts are created.

## Why start with Cargo, not rustc directly

You can compile a single file with `rustc`, but most real Rust projects need
dependency management, repeatable builds, and tests. Cargo standardizes that.

> [!IMPORTANT]
> In Academy, use Cargo for project work unless a lesson explicitly says otherwise.

## What you should see

You now have a project folder similar to this:

```text
hello-rust/
├── Cargo.toml
├── src/
│   └── main.rs
└── .gitignore
```

- `Cargo.toml` stores package metadata and dependencies.
- `src/main.rs` is the program entry point.
- `.gitignore` avoids committing build output.

---

## Recap

- `cargo new` creates a standard Rust project.
- `cargo run` compiles and runs your program in one command.
- Cargo layout is predictable and scales from small to large projects.

## Try it yourself

Edit `src/main.rs` to print your name, then run `cargo run` again.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Installing Rust](./01-installing-rust.md) | [Chapter 01](./README.md) · [Rust](../../README.md) · [Home](../../../../README.md) | [Rust Project Structure →](./03-rust-project-layout.md) |

</div>
