<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../README.md) / [Rust](../README.md) / [Reference](./README.md)

---

# Modules, Visibility, and Exports Reference

> Lookup for `mod`, `pub`, `use`, paths, and re-exports.

Full lesson: [Modules, Visibility, and Exports](../course/05-crates-modules-libraries-and-project-design/02-modules-and-visibility.md).

## Syntax Table

| Syntax | Description | Example |
|---|---|---|
| `mod name;` | Include a private module file. | `mod parser;` |
| `pub mod name;` | Include and expose a module. | `pub mod inventory;` |
| `use path::Item;` | Bring a name into local scope. | `use crate::inventory::Item;` |
| `pub use path::Item;` | Re-export a name publicly. | `pub use inventory::Item;` |
| `crate::` | Start path at current crate root. | `crate::report::format()` |
| `super::` | Start path at parent module. | `super::rules::validate()` |
| `self::` | Start path at current module. | `self::item::Item` |

## Prelude

The Rust prelude is the standard set of names automatically available in every
module.

Common prelude names:

| Name | What it is |
|---|---|
| `String` | owned, growable text |
| `Vec<T>` | growable list |
| `Option<T>` | value may be present or absent |
| `Result<T, E>` | operation may succeed or fail |
| `Box<T>` | owned heap allocation |
| `Clone`, `Copy`, `Debug`, `Default`, `Drop` | common traits |
| `Iterator`, `IntoIterator` | iteration traits |

Not in the prelude:

```rust
use std::collections::HashMap;
use std::fs;
use std::path::PathBuf;
```

Crates may expose their own prelude modules, but those are imported manually:

```rust
use some_crate::prelude::*;
```

Risk note: avoid wildcard prelude imports in library code unless the crate
documentation strongly recommends them. Specific imports make dependencies
clearer.

## File Layouts

Single-file module:

```text
src/
|-- lib.rs
`-- inventory.rs
```

```rust
// src/lib.rs
pub mod inventory;
```

Folder module:

```text
src/
|-- lib.rs
`-- inventory/
    |-- mod.rs
    |-- item.rs
    `-- rules.rs
```

```rust
// src/inventory/mod.rs
mod item;
mod rules;

pub use item::Item;
```

## Visibility

| Visibility | Who can access it | Use when |
|---|---|---|
| private | current module and children | default for implementation details |
| `pub` | anyone who can reach parent | part of public API |
| `pub(crate)` | current crate only | shared internal helper |
| `pub(super)` | parent module only | parent coordinates child modules |
| `pub(in crate::path)` | selected ancestor path | rare, strict internal boundary |

## Public Struct Patterns

Private fields, public constructor:

```rust
pub struct User {
    name: String,
}

impl User {
    pub fn new(name: impl Into<String>) -> Self {
        Self { name: name.into() }
    }

    pub fn name(&self) -> &str {
        &self.name
    }
}
```

Public fields:

```rust
pub struct Point {
    pub x: i32,
    pub y: i32,
}
```

Use public fields only when every possible value is valid and you are comfortable
making the field names part of your API.

## Risk Notes

| Pattern | Risk | Safer default |
|---|---|---|
| `pub mod everything;` | Exposes internal layout. | Re-export selected names with `pub use`. |
| public mutable fields | Callers can create invalid states. | private fields plus methods. |
| deep public paths | Hard to refactor later. | short public API in `lib.rs`. |
| `use super::*;` in production code | Hides dependencies. | Import specific names. |

## Cross References

- [Cargo Manifest and Config](./cargo-manifest-and-config.md)
- [Functions, Methods, Generics, and Traits](./functions-methods-generics-and-traits.md)
- [Errors, Warnings, and Debugging](./errors-warnings-and-debugging.md)
