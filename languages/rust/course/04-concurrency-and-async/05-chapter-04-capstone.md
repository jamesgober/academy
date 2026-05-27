<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 04](./README.md)

</div>

---

# Chapter 04 Capstone

> Apply concurrency and async concepts to a realistic design-level exercise.

## Project brief

Design and implement a small "job dispatcher" module with:

1. worker threads for CPU-bound tasks
2. channel-based message passing
3. explicit error types for job failures
4. timeout/cancellation policy document for async extension path

## Expected outputs

Your solution should include:
- clearly separated worker and coordination logic
- a diagram or notes describing message flow
- a documented cancellation/timeout policy

## Reviewer checklist

- Are synchronization choices explicit and justified?
- Is shared mutable state minimized?
- Is the async extension path documented clearly enough for a future implementation?

## Validation checklist

- no shared mutable data without synchronization
- thread joins handled safely
- error paths tested
- behavior documented for timeout and cancellation

## Next

Continue to crate and module design, then apply the same concurrency ideas to a
clean library/application split.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Async Error Handling, Timeouts, and Cancellation](./04-async-error-timeouts-and-cancellation.md) | [Chapter 04](./README.md) · [Rust](../../README.md) · [Home](../../../../README.md) | [Chapter 05 →](../05-crates-modules-libraries-and-project-design/README.md) |

</div>
