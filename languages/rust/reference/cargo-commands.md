<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../README.md) · [Track](../README.md) · [Reference Index](./README.md)

</div>

---

# Cargo Commands Reference

> Quick lookup. For full context, use [Chapter 01](../course/01-getting-started/).

## At a glance

| Command | Purpose | Typical use |
|---------|---------|-------------|
| `cargo new <name>` | Create project | Start new binary crate |
| `cargo run` | Build and run | Local dev loop |
| `cargo check` | Fast compile check | Frequent feedback |
| `cargo test` | Run tests | Validate behavior |
| `cargo fmt` | Format code | Style consistency |
| `cargo clippy` | Lint code | Catch common issues |

---

## Common command forms

```bash
cargo run
cargo check
cargo test
cargo add serde
cargo run -p cli
```

> [!TIP]
> Use `cargo check` more often than `cargo build` while iterating. It is usually faster.

> [!WARNING]
> `cargo clean` deletes build artifacts and can slow your next build significantly.
> Use it only when troubleshooting build-state issues.

---

<div align="center">

[← Reference Index](./README.md) · [Track](../README.md) · [Home](../../../README.md)

</div>
