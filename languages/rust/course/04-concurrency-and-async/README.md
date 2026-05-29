<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md)

---

# Chapter 04: Concurrency And Async

This chapter explains how Rust handles work that happens at the same time:
threads for parallel work, channels for message passing, shared state with
locks, and async for high-concurrency waiting.

By the end, you will be able to:

- Explain data races in plain language
- Read `Send` and `Sync` as safety capabilities
- Spawn and join threads
- Use `Arc<Mutex<T>>` when shared mutable state is appropriate
- Use channels to send results between workers
- Explain futures, runtimes, `.await`, timeouts, cancellation, and retries

## Lessons

| # | Lesson | Main skill |
|---|---|---|
| 01 | [Concurrency Safety In Rust](./01-concurrency-safety-in-rust.md) | Understand data-race prevention |
| 02 | [Threads, Shared State, And Channels](./02-threads-shared-state-and-channels.md) | Coordinate worker threads |
| 03 | [Async And Await Mental Model](./03-async-and-await-mental-model.md) | Understand futures and runtimes |
| 04 | [Async Error Handling, Timeouts, And Cancellation](./04-async-error-timeouts-and-cancellation.md) | Design reliable async flows |
| 05 | [Chapter 04 Capstone](./05-chapter-04-capstone.md) | Build a threaded job dispatcher |

---

[**Next ->** Concurrency Safety In Rust](./01-concurrency-safety-in-rust.md)  
[**<- Previous** Chapter 03: Testing, Edge Cases, And CI](../03-testing-edge-cases-and-ci/README.md)
