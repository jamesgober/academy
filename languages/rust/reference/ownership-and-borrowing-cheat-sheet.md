<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../README.md) / [Rust](../README.md) / [Reference](./README.md)

---

# Ownership and Borrowing Cheat Sheet

> Fast rules for Rust ownership-related compile errors.

## Ownership rules

- each value has one owner
- move transfers ownership
- owner drop frees resource

## Borrowing rules

- many `&T` or one `&mut T`
- no mutable + immutable borrows at same time
- references cannot outlive owned value

## Frequent fixes

- narrow borrow scope with inner block
- clone only when ownership transfer is truly needed
- pass `&str` instead of `String` where ownership is unnecessary

## Quick diagnostic prompts

- who owns this value now?
- is this borrow overlapping another borrow?
- is returned reference tied to valid input lifetime?

---

[Reference Index](./README.md) / [Rust](../README.md) / [Home](../../../README.md)