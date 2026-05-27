<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 03](./README.md)

</div>

---

# Chapter 03 Capstone

> Build a tested Rust module with CI gates and documented edge-case coverage.

## Requirements

1. Implement one non-trivial module with `Result`-based error handling.
2. Add unit tests for core logic.
3. Add integration tests for public API behavior.
4. Add at least 5 edge-case tests with explicit intent comments.
5. Add CI workflow for fmt + clippy + test.

## Expected outputs

Your project should demonstrate:
- passing local test suite
- CI status checks triggered from pull request
- at least one failing edge-case test before fix and a passing test after fix

## Reviewer checklist

- Are tests deterministic and readable?
- Are edge cases intentional rather than random?
- Does CI enforce the same quality gates locally and remotely?

## Submission checklist

- `cargo fmt --all -- --check` passes
- `cargo clippy --all-targets --all-features -- -D warnings` passes
- `cargo test --all-features` passes
- CI checks pass on pull request

> [!IMPORTANT]
> This capstone should be buildable from clean clone with documented commands.

---

## Next

Continue to [Chapter 04 — Concurrency and Async](../04-concurrency-and-async/README.md).

---

[**Next ->** Chapter Concurrency and Async](../04-concurrency-and-async/README.md)  
[**<- Previous** Debugging Test Failures and Flaky Behavior](./05-debugging-test-failures-and-flaky-behavior.md)
