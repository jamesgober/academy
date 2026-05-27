<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 05](./README.md)

</div>

---

# Library Crates: When and How to Build Them

> Build a library crate when the reusable logic is valuable beyond one binary.

**You will learn:**
- When a library crate is worth creating
- How to separate library and application code
- How to expose a stable public API

**Before this page, you should know:** [Modules and Visibility](./02-modules-and-visibility.md)

---

## When to make a library

Create a library crate when:
- multiple binaries share logic
- you want reuse across projects
- your code has a clear public API

## Layout pattern

```text
my-project/
├── Cargo.toml
└── src/
    ├── lib.rs
    └── main.rs
```

## Library API habits

- keep implementation details private
- document public functions
- return structured errors
- keep functions small and focused

> [!TIP]
> If your `main.rs` becomes a wall of logic, move reusable parts into `lib.rs`.

## Tiny library example

```rust
pub fn greet(name: &str) -> String {
    format!("Hello, {}", name)
}
```

---

## Recap

- Library crates are for reusable logic.
- Keep the public API small and documented.
- Move shared code out of binary entry points.

## Try it yourself

Take one helper function from a binary project and move it into a library crate.

---

[**Next ->** Binary Crates: Applications and Entry Points](./04-binary-crates-applications-and-entry-points.md)  
[**<- Previous** Modules, Visibility, and Exports](./02-modules-and-visibility.md)
