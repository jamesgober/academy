<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 02](./README.md)

</div>

---

# Variables, Mutability, and Shadowing

> Rust defaults to immutability to make state changes explicit.

**You will learn:**
- Rust variable declaration basics
- `mut` and when to use it
- Shadowing vs mutation

**Before this page, you should know:** [Chapter 01 — Getting Started](../01-getting-started/README.md)

---

## Immutability first

```rust
fn main() {
    let score = 10;
    // score = 20; // compile error
    println!("{}", score);
}
```

By default, variables cannot be reassigned.

## Mutable variables

```rust
fn main() {
    let mut score = 10;
    score = 20;
    println!("{}", score);
}
```

Use `mut` only when state must change.

## Shadowing

Shadowing creates a new variable with the same name:

```rust
fn main() {
    let speed = 40;
    let speed = speed + 10; // new binding
    println!("{}", speed);
}
```

This is different from mutation and often cleaner for transformations.

> [!TIP]
> Prefer immutable values and shadowing for step-by-step transformations.

---

## Recap

- Rust is immutable by default.
- `mut` enables reassignment.
- Shadowing creates a new binding and is not the same as mutation.

## Try it yourself

Write a program that starts with `let points = 50`, shadows it twice with
transformations, then prints the final value.

---

[**Next ->** Ownership: The One-Owner Rule](./02-ownership-the-one-owner-rule.md)  
[**<- Previous** Chapter Rust Core Mental Model](./README.md)
