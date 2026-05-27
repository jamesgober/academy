<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 05](./README.md)

</div>

---

# Crates and Cargo Packages

> A crate is a compilation unit. A Cargo package is the thing you publish and build.

**You will learn:**
- What a crate is
- How crates relate to Cargo packages
- How to choose crate boundaries early

**Before this page, you should know:** [Chapter 04 Capstone](../04-concurrency-and-async/05-chapter-04-capstone.md)

---

## Crate in plain language

A **crate** is a Rust compilation unit.
It can be a library crate or a binary crate.

## Package in plain language

A **package** is a Cargo-managed project that can contain one or more crates.

```text
Cargo package
    |
    |-- library crate  -> src/lib.rs
    `-- binary crate   -> src/main.rs or src/bin/*.rs
```

## Why this matters

Understanding the distinction helps you design project layout deliberately.

- library logic belongs in library crates
- executable behavior belongs in binary crates

> [!TIP]
> Keep one job per crate when possible. It makes code easier to navigate and test.

---

## Recap

- Crate = compilation unit.
- Package = Cargo-managed project.
- Crate boundaries should reflect responsibility.

## Try it yourself

Explain the difference between a crate and a package using one sentence each,
then map a sample project into lib/bin crates.

---

[**Next ->** Modules, Visibility, and Exports](./02-modules-and-visibility.md)  
[**<- Previous** Chapter Concurrency and Async](../04-concurrency-and-async/README.md)
