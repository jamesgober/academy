<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 05](./README.md)

</div>

---

# Chapter 05 Project Tutorials

> Finish with two guided project tutorials: one library and one application.

## Tutorial 1: Build a reusable library

Create a library crate for a small domain such as:
- scoreboard rules
- string formatting helpers
- inventory calculations

### Expected outputs

- clear `lib.rs`
- small public API
- tests for public behavior
- documentation comments on public items

## Tutorial 2: Build a thin application

Create a binary crate that:
- uses the library crate
- parses input
- prints results
- keeps `main.rs` small

### Expected outputs

- reusable logic lives in library
- `main.rs` stays thin
- build/test commands documented

## Reviewer checklist

- Is crate boundary obvious?
- Is public API intentionally small?
- Can tests run without manual setup?
- Does the application depend on library code cleanly?

> [!IMPORTANT]
> Project tutorials should reinforce principles from the chapter, not introduce
> hidden new architecture rules.

---

## Next

Continue back through the Rust reference or apply these patterns to your own
project.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Modular Design and Codebase Cleanliness](./05-modular-design-and-codebase-cleanliness.md) | [Chapter 05](./README.md) · [Rust](../../README.md) · [Home](../../../../README.md) | [Rust Reference →](../../reference/README.md) |

</div>
