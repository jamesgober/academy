<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 05](./README.md)

---

# Modules, Visibility, and Exports

> Rust modules answer three questions: where code lives, who can see it, and what name callers use.

**You will learn:**
- how `mod`, `pub`, `use`, and `pub use` work together
- how to split code across files without confusing the compiler
- how to expose a clean public API while hiding implementation details
- how a binary target uses a library target from the same package
- how to read common module errors

**Before this page, you should know:** [Crates and Cargo Packages](./01-crates-and-cargo-packages.md)

---

## The Beginner Mental Model

A module is a named area of code. Modules organize names and control privacy.

Rust starts at a crate root:

- `src/main.rs` is the crate root for a binary crate.
- `src/lib.rs` is the crate root for a library crate.

From that root, `mod inventory;` means "compile the module named `inventory` as
part of this crate." Rust then looks for one of these files:

```text
src/inventory.rs
src/inventory/mod.rs
```

For new projects, prefer `src/inventory.rs` until the module needs child files.

## Build a Real Cross-File Example

Create a library-style project:

```bash
cargo new inventory-masterclass
cd inventory-masterclass
```

Add `src/lib.rs`:

```rust
pub mod inventory;
pub mod report;
```

This says the crate has two public modules: `inventory` and `report`.

Create `src/inventory.rs`:

```rust
#[derive(Debug, Clone, PartialEq, Eq)]
pub struct Item {
    name: String,
    count: u32,
}

impl Item {
    pub fn new(name: impl Into<String>, count: u32) -> Self {
        Self {
            name: name.into(),
            count,
        }
    }

    pub fn name(&self) -> &str {
        &self.name
    }

    pub fn count(&self) -> u32 {
        self.count
    }

    pub fn add_stock(&mut self, amount: u32) {
        self.count += amount;
    }
}
```

Important details:

- `pub struct Item` makes the type visible outside the module.
- `name` and `count` are private fields because they do not have `pub`.
- `Item::new` is public, so callers can create valid items.
- Getter methods expose read access without allowing random field mutation.
- `&self` borrows the item for reading.
- `&mut self` borrows the item for mutation.

This is a real Rust design habit: expose behavior, not every internal field.

Create `src/report.rs`:

```rust
use crate::inventory::Item;

pub fn format_item(item: &Item) -> String {
    format!("{}: {}", item.name(), item.count())
}
```

`crate::inventory::Item` means "start from this crate's root, then go into the
`inventory` module, then use `Item`."

Create `src/main.rs`:

```rust
use inventory_masterclass::inventory::Item;
use inventory_masterclass::report::format_item;

fn main() {
    let mut notebook = Item::new("notebook", 3);
    notebook.add_stock(2);

    println!("{}", format_item(&notebook));
}
```

Run:

```bash
cargo run
```

Expected output:

```text
notebook: 5
```

## Why `main.rs` Can Use the Library Name

When a package has both `src/main.rs` and `src/lib.rs`, Cargo builds two crates:

- a binary crate from `main.rs`
- a library crate from `lib.rs`

The binary can import the library by crate name:

```rust
use inventory_masterclass::inventory::Item;
```

If the package name has hyphens, the crate name uses underscores:

```toml
[package]
name = "inventory-masterclass"
```

```rust
use inventory_masterclass::inventory::Item;
```

## `mod`, `pub`, `use`, and `pub use`

These keywords are related but not interchangeable.

| Syntax | Meaning | Beginner translation |
|---|---|---|
| `mod inventory;` | Include a module in this crate. | "Compile this file as part of my project." |
| `pub mod inventory;` | Include it and expose the module. | "Other modules/crates may enter this module." |
| `pub struct Item` | Expose a type. | "Callers may name this type." |
| `pub fn new(...)` | Expose a function or method. | "Callers may call this behavior." |
| `use crate::inventory::Item;` | Bring a path into local scope. | "Let me write `Item` instead of the full path." |
| `pub use inventory::Item;` | Re-export a name. | "Let callers use a shorter public path." |

## The Prelude: Why Some Names Need No Import

The Rust prelude is a small set of names that Rust automatically brings into
every module. A prelude is just an automatic starter import list.

That is why you can write common names like these without adding `use` lines:

```rust
let mut numbers = Vec::new();
let name = String::from("Ada");
let maybe_name: Option<String> = Some(name);
let parsed: Result<i32, _> = "42".parse();
```

You did not import `Vec`, `String`, `Option`, or `Result` because they come from
the standard prelude.

The prelude does not include everything. For example, `HashMap` needs an import:

```rust
use std::collections::HashMap;

fn main() {
    let mut counts = HashMap::new();
    counts.insert("notebook", 3);
}
```

Use this rule: if a type or trait is not found, check whether it needs a `use`
statement. Compiler suggestions often show the exact import.

> [!NOTE]
> External crates can also provide a `prelude` module by convention, often named
> `crate_name::prelude`. Those are not automatic. You import them yourself when
> the crate documentation recommends it.

## Re-Export for a Cleaner API

Right now callers write:

```rust
use inventory_masterclass::inventory::Item;
use inventory_masterclass::report::format_item;
```

That exposes your internal module layout. If you later rename `report`, callers
would have to change imports.

You can re-export important public names from `src/lib.rs`:

```rust
pub mod inventory;
pub mod report;

pub use inventory::Item;
pub use report::format_item;
```

Now `src/main.rs` can use:

```rust
use inventory_masterclass::{format_item, Item};

fn main() {
    let item = Item::new("pen", 12);
    println!("{}", format_item(&item));
}
```

This is what "exporting" usually means in Rust library design: make selected
items public and optionally re-export them from a convenient path.

> [!IMPORTANT]
> `pub use` does not duplicate code. It creates a public shortcut to an item.

## Visibility Options You Will See

| Visibility | Who can use it |
|---|---|
| private | Only the current module and its child modules. |
| `pub` | Anyone who can reach the parent module. |
| `pub(crate)` | Anywhere inside the current crate, but not external users. |
| `pub(super)` | The parent module. |
| `pub(in crate::path)` | A specific ancestor path. |

Most beginner and intermediate code uses private, `pub`, and `pub(crate)`.

Use `pub(crate)` for helper APIs that are useful across your crate but should
not become promises to external users.

## Split a Module Into Child Files

If `inventory.rs` grows, convert it into a folder:

```text
src/
|-- lib.rs
|-- inventory/
|   |-- mod.rs
|   |-- item.rs
|   `-- rules.rs
`-- report.rs
```

`src/inventory/mod.rs`:

```rust
mod item;
mod rules;

pub use item::Item;
pub(crate) use rules::can_add_stock;
```

`src/inventory/item.rs`:

```rust
use super::rules::can_add_stock;

#[derive(Debug, Clone, PartialEq, Eq)]
pub struct Item {
    name: String,
    count: u32,
}

impl Item {
    pub fn new(name: impl Into<String>, count: u32) -> Self {
        Self {
            name: name.into(),
            count,
        }
    }

    pub fn add_stock(&mut self, amount: u32) {
        if can_add_stock(amount) {
            self.count += amount;
        }
    }
}
```

`src/inventory/rules.rs`:

```rust
pub(crate) fn can_add_stock(amount: u32) -> bool {
    amount > 0 && amount <= 10_000
}
```

`super::rules` means "go up to the parent module, then into `rules`."

## Common Module Errors

### `file not found for module`

Example:

```text
error[E0583]: file not found for module `inventory`
```

Cause: you wrote `mod inventory;`, but Rust could not find `inventory.rs` or
`inventory/mod.rs` next to the crate root or parent module.

Fix: create the file at the path Rust expects, or correct the module name.

### `module is private`

Example:

```text
error[E0603]: module `inventory` is private
```

Cause: another module or crate tried to access a module declared with `mod`
instead of `pub mod`.

Fix: make the module public only if it should be part of the API:

```rust
pub mod inventory;
```

### `field is private`

Example:

```text
error[E0616]: field `count` of struct `Item` is private
```

Cause: the type is public, but the field is not.

Fix options:

- add a public getter such as `count()`
- add a public method that performs a safe mutation
- make the field public only for simple data structs where any value is valid

## Design Rule for Real Projects

Use this default:

```rust
mod implementation_detail;
pub mod public_area;
pub use public_area::ImportantType;
```

Make code public because a caller needs it, not because it is convenient during
development. Public API becomes a promise.

---

## Recap

- `mod` includes a module in the crate.
- `pub` exposes modules, types, functions, methods, and fields.
- `use` shortens paths inside one file.
- `pub use` re-exports a public name for callers.
- `main.rs` can import `lib.rs` through the package's crate name.
- Private fields plus public methods are a common Rust design pattern.

## Try It Yourself

Create a package named `gradebook-cli` with:

- `src/lib.rs`
- `src/student.rs`
- `src/report.rs`
- `src/main.rs`

Expose `Student` and `format_student_report` from `lib.rs` with `pub use`.
Keep at least one struct field private and provide a getter method.

---

[**Next ->** Library Crates: When and How to Build Them](./03-library-crates-when-and-how-to-build-them.md)  
[**<- Previous** Crates and Cargo Packages](./01-crates-and-cargo-packages.md)
