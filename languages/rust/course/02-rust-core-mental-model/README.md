<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md)

---

# Chapter 02: Rust Core Mental Model

This chapter teaches the concepts that make Rust feel different: ownership,
borrowing, lifetimes, data modeling, and explicit error handling.

By the end, you will be able to:

- Explain why Rust values have owners
- Decide when to pass by value, shared reference, or mutable reference
- Read common borrow checker errors without panic
- Use lifetimes as relationship labels
- Model data with structs and enums
- Use `Option` and `Result` instead of guessing or panicking

> [!IMPORTANT]
> Ownership, borrowing, and lifetimes are not side quests. They are the core
> model Rust uses to guarantee memory safety.

## Lessons

| # | Lesson | Main skill |
|---|---|---|
| 01 | [Variables, Mutability, and Shadowing](./01-variables-mutability-and-shadowing.md) | Bind names and change values intentionally |
| 02 | [Ownership: The One-Owner Rule](./02-ownership-the-one-owner-rule.md) | Understand moves, copies, drops, and clones |
| 03 | [Borrowing and References](./03-borrowing-and-references.md) | Share or mutate values safely |
| 04 | [Lifetimes in Plain Language](./04-lifetimes-in-plain-language.md) | Read lifetime relationships |
| 05 | [Structs, Enums, and Pattern Matching](./05-structs-enums-and-pattern-matching.md) | Build useful data models |
| 06 | [Error Handling with Option and Result](./06-error-handling-with-option-and-result.md) | Handle absence and failure explicitly |
| 07 | [Chapter 02 Checkpoint](./07-chapter-02-checkpoint.md) | Build a garage intake module |

---

[**Next ->** Variables, Mutability, And Shadowing](./01-variables-mutability-and-shadowing.md)  
[**<- Previous** Chapter 01: Getting Started](../01-getting-started/README.md)
