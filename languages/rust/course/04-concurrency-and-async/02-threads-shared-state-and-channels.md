<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 04](./README.md)

</div>

---

# Threads, Shared State, and Channels

> Rust gives two main concurrency patterns: shared state and message passing.

**You will learn:**
- `std::thread` basics
- shared state with `Arc<Mutex<T>>`
- channels for message passing

**Before this page, you should know:** [Concurrency Safety in Rust](./01-concurrency-safety-in-rust.md)

---

## Thread basics

```rust
use std::thread;

fn main() {
    let handle = thread::spawn(|| {
        println!("worker thread");
    });

    handle.join().unwrap();
}
```

## Shared state

Use `Arc<Mutex<T>>` when multiple threads need shared mutable data.

## Channels

Use channels when you prefer passing messages instead of sharing mutable state.

```rust
use std::sync::mpsc;
use std::thread;

fn main() {
    let (tx, rx) = mpsc::channel();

    thread::spawn(move || {
        tx.send("done").unwrap();
    });

    let msg = rx.recv().unwrap();
    println!("{}", msg);
}
```

> [!TIP]
> Prefer message passing first. Shared mutable state should be deliberate.

---

## Recap

- Threads run work in parallel.
- `Arc<Mutex<T>>` enables shared mutable state with synchronization.
- Channels reduce coupling and simplify reasoning.

## Try it yourself

Create two worker threads that send progress messages to main thread via channel.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Concurrency Safety in Rust](./01-concurrency-safety-in-rust.md) | [Chapter 04](./README.md) · [Rust](../../README.md) · [Home](../../../../README.md) | [Async and Await Mental Model →](./03-async-and-await-mental-model.md) |

</div>
