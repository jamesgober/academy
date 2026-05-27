<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 05](./README.md)

</div>

---

# Modules and Visibility

> Modules organize code; visibility controls what the outside world can touch.

**You will learn:**
- `mod`, `pub`, and module layout
- How visibility shapes public API design
- How modules help keep codebases clean

**Before this page, you should know:** [Crates and Cargo Packages](./01-crates-and-cargo-packages.md)

---

## Module structure

```rust
mod car;

fn main() {
    println!("module demo");
}
```

Modules can live in file trees and help split logic by responsibility.

## Visibility

- `pub` exposes an item outside the module.
- private items stay internal by default.

## Clean design principle

Expose a small public surface, keep implementation details private.

> [!IMPORTANT]
> Good modular design makes refactoring easier and reduces accidental coupling.

---

## Recap

- Modules organize code into meaningful units.
- Visibility determines public API surface.
- Smaller public surfaces are easier to maintain.

## Try it yourself

Design a module tree for a car app with `engine`, `display`, and `storage`
modules. Mark only what must be public.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Crates and Cargo Packages](./01-crates-and-cargo-packages.md) | [Chapter 05](./README.md) · [Rust](../../README.md) · [Home](../../../../README.md) | [Library Crates: When and How to Build Them →](./03-library-crates-when-and-how-to-build-them.md) |

</div>
