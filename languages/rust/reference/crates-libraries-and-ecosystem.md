<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../README.md) / [Rust](../README.md) / [Reference](./README.md)

---

# Crates, Libraries, and Ecosystem Lookup

> Rust libraries are usually distributed as crates. Know where to find them, how to read them, and when to be cautious.

## Where to Find Libraries

| Site | Use it for |
|---|---|
| [crates.io](https://crates.io/) | Search packages and versions. |
| [docs.rs](https://docs.rs/) | Read generated API documentation. |
| Project repository | Check source, issues, examples, and maintenance. |
| The Rust Book | Learn official language concepts. |
| The Cargo Book | Learn Cargo, manifests, workspaces, publishing, and config. |

## Add a Crate

```bash
cargo add serde --features derive
cargo add anyhow
cargo add clap --features derive
```

Equivalent `Cargo.toml`:

```toml
[dependencies]
serde = { version = "1", features = ["derive"] }
anyhow = "1"
clap = { version = "4", features = ["derive"] }
```

## Evaluate a Crate Before Using It

| Check | Why it matters |
|---|---|
| docs exist | You need examples and API descriptions. |
| recent releases | Maintenance signal, not a guarantee. |
| license | Must be compatible with your project. |
| dependency count | More transitive code means more audit surface. |
| examples | Shows intended usage. |
| issue tracker | Reveals active bugs and maintainer responsiveness. |
| unsafe usage | Not automatically bad, but deserves more care. |

## Common Crate Categories

| Need | Common crates to research |
|---|---|
| CLI parsing | `clap`, `argh` |
| error handling | `anyhow`, `thiserror` |
| serialization | `serde`, `serde_json`, `toml` |
| async runtime | `tokio`, `async-std` |
| HTTP client | `reqwest`, `ureq` |
| logging/tracing | `tracing`, `log`, `env_logger` |
| testing helpers | `pretty_assertions`, `tempfile`, `assert_cmd` |

These are starting points, not automatic recommendations. Check the current docs
and project fit before adding dependencies.

## Risk Notes

| Situation | Watch for |
|---|---|
| unmaintained crate | security and compatibility issues |
| tiny helper dependency | maybe write it yourself if the behavior is trivial |
| async dependency | may require a specific runtime or feature flags |
| default features | can pull in heavier dependencies than expected |
| pre-1.0 version | API may change more often |

## Cross References

- [Cargo Manifest and Config](./cargo-manifest-and-config.md)
- [Cargo Commands](./cargo-commands.md)
- [Modules, Visibility, and Exports](./modules-visibility-and-exports.md)
