<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 05](./README.md)

</div>

---

# Binary Crates: Applications and Entry Points

> Binary crates are the application layer: they parse input, call logic, and present output.

**You will learn:**
- What a binary crate is
- How `main` fits application structure
- How to keep application logic thin

**Before this page, you should know:** [Library Crates: When and How to Build Them](./03-library-crates-when-and-how-to-build-them.md)

---

## Entry point responsibilities

The binary crate should usually:
- parse command-line input
- call into library code
- print results or errors

## Good application split

```text
src/
├── lib.rs   reusable logic
└── main.rs  thin entry point
```

## Why thin binaries matter

They make testing easier and keep business logic reusable.

> [!IMPORTANT]
> If `main.rs` contains most of the logic, consider extracting a library crate.

## Mini example

```rust
fn main() {
    let result = important_logic::run();
    println!("{}", result);
}
```

---

## Recap

- Binary crates are executable applications.
- Keep `main` focused on orchestration.
- Put reusable logic in library crates.

## Try it yourself

Refactor a large `main` function into `main.rs` + `lib.rs` split.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Library Crates: When and How to Build Them](./03-library-crates-when-and-how-to-build-them.md) | [Chapter 05](./README.md) · [Rust](../../README.md) · [Home](../../../../README.md) | [Modular Design and Codebase Cleanliness →](./05-modular-design-and-codebase-cleanliness.md) |

</div>
