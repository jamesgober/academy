<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 04](./README.md)

</div>

---

# Concurrency Safety in Rust

> Rust's ownership and borrowing rules extend into concurrency to prevent data races.

**You will learn:**
- Why data races happen
- How Rust's type system blocks them
- `Send` and `Sync` intuition

**Before this page, you should know:** [Chapter 03 Capstone](../03-testing-edge-cases-and-ci/06-chapter-03-capstone.md)

---

## Data race in plain language

A data race occurs when:
- two threads access same data
- at least one thread writes
- no synchronization exists

## Rust's safety model

Rust enforces aliasing rules that make unsynchronized mutable sharing hard to
express safely.

```text
Shared data
    |
    |-- many readers allowed
    |       `-- no writer at the same time
    |
    `-- one writer allowed
            `-- no readers at the same time
```

## `Send` and `Sync` intuition

- `Send`: value can move to another thread safely.
- `Sync`: references to value can be shared across threads safely.

> [!IMPORTANT]
> You usually do not implement these traits manually. Standard library types
> compose them correctly for common use cases.

---

## Recap

- Rust treats concurrency safety as a compile-time concern.
- Ownership/borrowing rules reduce race-prone patterns.
- `Send` and `Sync` describe thread-safety capabilities.

## Try it yourself

Explain whether `String` and `&String` are safe to transfer/share between
threads and why.

---

[**Next ->** Threads, Shared State, and Channels](./02-threads-shared-state-and-channels.md)  
[**<- Previous** Chapter Concurrency and Async](./README.md)
