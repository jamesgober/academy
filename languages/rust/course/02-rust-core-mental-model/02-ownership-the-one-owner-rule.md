<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 02](./README.md)

</div>

---

# Ownership: The One-Owner Rule

> Each value in Rust has exactly one owner at a time.

**You will learn:**
- What ownership means
- Move semantics for heap data
- Why ownership prevents use-after-free bugs

**Before this page, you should know:** [Variables, Mutability, and Shadowing](./01-variables-mutability-and-shadowing.md)

---

## Core ownership rules

1. Every value has one owner.
2. Ownership can move to another variable.
3. When owner goes out of scope, value is dropped.

## Move example

```rust
fn main() {
    let car_name = String::from("Comet");
    let copied = car_name; // move occurs
    // println!("{}", car_name); // compile error
    println!("{}", copied);
}
```

`String` owns heap memory, so assignment moves ownership.

## Backend intuition

Rust tracks ownership at compile time, not runtime GC.

```text
Before assignment:
car_name ---> String data on the heap

After `let copied = car_name;`:
copied   ---> String data on the heap
car_name -X-> cannot be used anymore
```

The compiler blocks access through invalid owners.

> [!NOTE]
> Common compiler translation:
> - `E0382` usually means you tried to use a value after ownership moved.
> - Fix by using the new owner, borrowing instead of moving, or cloning only
>   when duplication is actually needed.

## Copy types vs move types

Small fixed-size types like `i32` are copied by value.
Heap-owning types like `String` are moved unless cloned.

```rust
fn main() {
    let hp = 100;
    let hp2 = hp; // copy
    println!("{} {}", hp, hp2);
}
```

> [!IMPORTANT]
> Ownership is a compile-time safety model, not a style preference.

---

## Recap

- Ownership ensures only one active owner for each value.
- Heap-owning values move unless explicitly cloned.
- Compile-time ownership checks prevent memory hazards.

## Try it yourself

Write one example using `i32` copy and one using `String` move, then explain
why one works after assignment and the other does not.

## Reviewer checklist

- Can the learner explain move semantics in their own words?
- Do they understand why `String` behaves differently from `i32`?
- Can they point to the exact line where ownership moved?

---

[**Next ->** Borrowing and References](./03-borrowing-and-references.md)  
[**<- Previous** Variables, Mutability, and Shadowing](./01-variables-mutability-and-shadowing.md)
