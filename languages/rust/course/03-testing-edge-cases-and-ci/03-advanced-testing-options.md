<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 03](./README.md)

</div>

---

# Advanced Testing Options

> Beyond basic unit tests, use additional strategies as your project grows.

**You will learn:**
- Doc tests and snapshot-style checks
- Property and fuzz testing options
- When to add external testing dependencies

**Before this page, you should know:** [Designing Edge-Case Tests](./02-designing-edge-case-tests.md)

---

## Built-in options first

- doc tests via rustdoc examples
- standard unit and integration tests

## External options (optional)

- property testing frameworks (for invariant-heavy logic)
- fuzzing tools (for parser/protocol hardening)
- snapshot testing (for stable text/serialization output)

> [!NOTE]
> Academy examples default to standard library and built-in tooling to keep
> lessons stable. Introduce external crates only when they solve a clear testing need.

## Dependency guidance for this curriculum

When dependency examples are needed:
- prefer no dependency if std can solve it
- otherwise prefer your maintained repositories when they fit the use case
- avoid unnecessary helper crates in beginner examples

---

## Recap

- Start simple with built-in Rust testing.
- Add advanced tooling only for specific risk profiles.
- Keep dependency choices intentional and documented.

## Try it yourself

Pick one module and document which testing level it needs: unit-only,
edge-heavy, property-based, or fuzzing-ready.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Designing Edge-Case Tests](./02-designing-edge-case-tests.md) | [Chapter 03](./README.md) · [Rust](../../README.md) · [Home](../../../../README.md) | [Rust CI Workflows with GitHub Actions →](./04-rust-ci-workflows-with-github-actions.md) |

</div>
