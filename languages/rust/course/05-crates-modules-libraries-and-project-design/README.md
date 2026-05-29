<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md)

---

# Chapter 05: Crates, Modules, Libraries, And Project Design

This chapter teaches how Rust projects are organized: packages, crates, modules,
visibility, library APIs, binary entry points, and clean boundaries.

By the end, you will be able to:

- Explain package, crate, and module differences
- Read and design `Cargo.toml` layouts
- Export modules across files with `mod`, `pub`, `use`, and `pub use`
- Understand the Rust prelude
- Split reusable library logic from application wiring
- Keep public APIs smaller than internal file structure

> [!IMPORTANT]
> Clean Rust architecture starts with small modules, clear public APIs, and
> deliberate crate boundaries.

## Lessons

| # | Lesson | Main skill |
|---|---|---|
| 01 | [Crates And Cargo Packages](./01-crates-and-cargo-packages.md) | Understand package and crate structure |
| 02 | [Modules, Visibility, And Exports](./02-modules-and-visibility.md) | Use cross-file modules and re-exports |
| 03 | [Library Crates: When And How To Build Them](./03-library-crates-when-and-how-to-build-them.md) | Design reusable APIs |
| 04 | [Binary Crates: Applications And Entry Points](./04-binary-crates-applications-and-entry-points.md) | Keep apps thin and testable |
| 05 | [Modular Design And Codebase Cleanliness](./05-modular-design-and-codebase-cleanliness.md) | Refactor boundaries cleanly |
| 06 | [Project Tutorial: Inventory CLI With A Reusable Library](./06-chapter-05-project-tutorials.md) | Build a structured Rust project |

---

[**Next ->** Crates And Cargo Packages](./01-crates-and-cargo-packages.md)  
[**<- Previous** Chapter 04: Concurrency And Async](../04-concurrency-and-async/README.md)
