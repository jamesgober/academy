<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../README.md) · [Track](../README.md) · [Reference Index](./README.md)

</div>

---

# Environment Variables and Cargo Config

> Quick lookup for Rust runtime environment variables and Cargo build knobs.

## Common runtime environment variables

Use these from application code with `std::env`.

| Variable | Typical purpose |
|----------|-----------------|
| `PATH` | Find executables on the system |
| `HOME` / `USERPROFILE` | Locate the user's home directory |
| `RUST_LOG` | Control logging verbosity in apps that use logging crates |
| `APP_ENV` | Application-specific environment mode |

## Common Cargo and Rust build variables

| Variable | Typical purpose |
|----------|-----------------|
| `CARGO_HOME` | Cargo cache/config location |
| `CARGO_TARGET_DIR` | Override build output directory |
| `RUSTFLAGS` | Pass extra compiler flags |
| `RUSTDOCFLAGS` | Pass extra rustdoc flags |
| `CARGO_NET_OFFLINE` | Force Cargo offline mode |

## Practical guidance

- Prefer environment variables for configuration that changes per machine or deployment.
- Keep secrets out of source control.
- Use `cargo run` or `cargo test` with env vars when you need local overrides.

## Examples

```bash
set RUST_LOG=debug
set CARGO_TARGET_DIR=target-local
cargo test
```

```rust
use std::env;

let mode = env::var("APP_ENV").unwrap_or_else(|_| "development".to_string());
```

---

<div align="center">

[← Reference Index](./README.md) · [Track](../README.md) · [Home](../../../README.md)

</div>
