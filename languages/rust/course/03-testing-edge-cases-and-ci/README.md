<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md)

---

# Chapter 03: Testing, Edge Cases, And CI

This chapter turns "it works once" into "we can trust this repeatedly." You will
test small rules, public APIs, documentation examples, edge cases, and CI gates.

By the end, you will be able to:

- Write unit tests and integration tests
- Design edge-case tests before bugs escape
- Use doc tests for public examples
- Choose when advanced testing tools are worth it
- Add a Rust GitHub Actions workflow
- Debug test failures and flaky behavior

> [!IMPORTANT]
> Every production-facing Rust project should have local test commands and CI
> checks that gate merges.

## Lessons

| # | Lesson | Main skill |
|---|---|---|
| 01 | [Rust Testing Foundations](./01-rust-testing-foundations.md) | Write and run basic tests |
| 02 | [Designing Edge-Case Tests](./02-designing-edge-case-tests.md) | Find boundaries and failure modes |
| 03 | [Advanced Testing Options](./03-advanced-testing-options.md) | Use doc, ignored, property, fuzz, and snapshot ideas |
| 04 | [Rust CI Workflows With GitHub Actions](./04-rust-ci-workflows-with-github-actions.md) | Automate quality gates |
| 05 | [Debugging Test Failures And Flaky Behavior](./05-debugging-test-failures-and-flaky-behavior.md) | Diagnose failures calmly |
| 06 | [Chapter 03 Capstone](./06-chapter-03-capstone.md) | Build a tested Study Log library |

---

[**Next ->** Rust Testing Foundations](./01-rust-testing-foundations.md)  
[**<- Previous** Chapter 02: Rust Core Mental Model](../02-rust-core-mental-model/README.md)
