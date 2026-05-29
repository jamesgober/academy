<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../README.md) / [Rust](../README.md) / [Reference](./README.md)

---

# Environment Variables And Cargo Config Reference

> Lookup for runtime environment variables, Cargo environment variables, and
> `.cargo/config.toml` settings.

Related lessons:

- [Installing Rust](../course/01-getting-started/01-installing-rust.md)
- [Cargo Workflow Essentials](../course/01-getting-started/04-cargo-workflow.md)
- [Files, CLI Input, Environment, And Useful Programs](../course/06-practical-rust-mastery/04-files-cli-input-environment-and-useful-programs.md)

---

## Read Environment Variables In Rust

```rust
use std::env;

fn app_mode() -> String {
    env::var("APP_ENV").unwrap_or_else(|_| "development".to_string())
}
```

Fallible version:

```rust
use std::env;

fn required_api_key() -> Result<String, env::VarError> {
    env::var("API_KEY")
}
```

`std::env::var` returns `Result<String, VarError>` because the variable may be
missing or invalid Unicode.

---

## Set Variables For One Command

PowerShell:

```powershell
$env:APP_ENV = "development"
cargo run
```

Bash:

```bash
APP_ENV=development cargo run
```

PowerShell variable values remain in that shell session until changed or removed:

```powershell
Remove-Item Env:APP_ENV
```

---

## Common Runtime Variables

| Variable | Typical purpose | Notice |
|---|---|---|
| `PATH` | Search path for commands | Setup-sensitive |
| `HOME` / `USERPROFILE` | User home directory | OS differs |
| `APP_ENV` | App-specific mode | You define meaning |
| `RUST_LOG` | Logging level for apps using logging crates | Requires logging setup |
| `NO_COLOR` | Disable colored output | Convention, not Rust-specific |

Never commit secrets such as API keys into source control.

---

## Cargo And Rust Variables

| Variable | Purpose | Example |
|---|---|---|
| `CARGO_HOME` | Cargo cache/config location | Custom tool cache |
| `RUSTUP_HOME` | rustup toolchain location | Custom toolchain storage |
| `CARGO_TARGET_DIR` | Build output directory | Shared or temporary target dir |
| `RUSTFLAGS` | Extra compiler flags | CI or special builds |
| `RUSTDOCFLAGS` | Extra rustdoc flags | Docs warnings |
| `CARGO_NET_OFFLINE` | Offline dependency mode | `true` |
| `RUST_BACKTRACE` | Panic backtraces | `1` or `full` |

Example:

```bash
RUST_BACKTRACE=1 cargo test failing_case
```

PowerShell:

```powershell
$env:RUST_BACKTRACE = "1"
cargo test failing_case
```

---

## `.cargo/config.toml`

Project-local config path:

```text
.cargo/config.toml
```

Example:

```toml
[alias]
q = "check"
t = "test"

[build]
target-dir = "target-local"

[env]
APP_ENV = "development"
```

Then:

```bash
cargo q
cargo t
```

Use aliases sparingly in teaching material. Normal Cargo commands are clearer
for beginners.

---

## Toolchain Pinning

`rust-toolchain.toml`:

```toml
[toolchain]
channel = "stable"
components = ["rustfmt", "clippy"]
```

Use when a project needs contributors and CI to use the same toolchain channel
and components.

Risk:

```text
Over-pinning can make updates harder.
Under-pinning can make builds differ across machines.
```

For learning projects, stable is usually enough.

---

## Configuration Decision Model

| Need | Prefer |
|---|---|
| Value changes per machine | Environment variable |
| Secret value | Secret manager or environment variable, never Git |
| Cargo command shortcut | `.cargo/config.toml` alias |
| Shared dependency/build behavior | `Cargo.toml` |
| Shared toolchain channel | `rust-toolchain.toml` |
| One-off debugging | Temporary shell variable |

---

[Reference Index](./README.md) / [Rust](../README.md) / [Home](../../../README.md)
