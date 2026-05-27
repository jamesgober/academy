<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 05](./README.md)

---

# Project Tutorial: Inventory CLI with a Reusable Library

> Build a small real-world Rust project that separates reusable domain logic from command-line input and output.

This tutorial is intentionally more complete than a toy exercise. You will build
the shape of a maintainable Rust project: library code, binary code, modules,
tests, examples, and documented commands.

## Final Project Shape

```text
inventory-cli/
|-- Cargo.toml
|-- README.md
|-- src/
|   |-- lib.rs
|   |-- inventory.rs
|   |-- report.rs
|   `-- main.rs
|-- tests/
|   `-- inventory_flow.rs
`-- examples/
    `-- print_report.rs
```

The project will let you create inventory items, add stock, reject invalid
updates, and print a small report.

## Step 1: Create the Project

```bash
cargo new inventory-cli
cd inventory-cli
```

Update `Cargo.toml`:

```toml
[package]
name = "inventory-cli"
version = "0.1.0"
edition = "2024"
description = "A beginner-friendly Rust inventory CLI project."
license = "MIT OR Apache-2.0"

[dependencies]
```

Only `name` is strictly required for Cargo, but real projects should include
clear metadata. The `edition` field keeps language behavior explicit.

## Step 2: Create the Library Root

Create `src/lib.rs`:

```rust
pub mod inventory;
pub mod report;

pub use inventory::{InventoryError, Item};
pub use report::format_item_report;
```

This file is the public front door of the library. The `pub use` lines create
short imports for callers:

```rust
use inventory_cli::{format_item_report, Item};
```

## Step 3: Model Inventory Data

Create `src/inventory.rs`:

```rust
#[derive(Debug, Clone, PartialEq, Eq)]
pub struct Item {
    name: String,
    count: u32,
}

#[derive(Debug, Clone, PartialEq, Eq)]
pub enum InventoryError {
    EmptyName,
    ZeroChange,
}

impl Item {
    pub fn new(name: impl Into<String>, count: u32) -> Result<Self, InventoryError> {
        let name = name.into();

        if name.trim().is_empty() {
            return Err(InventoryError::EmptyName);
        }

        Ok(Self { name, count })
    }

    pub fn name(&self) -> &str {
        &self.name
    }

    pub fn count(&self) -> u32 {
        self.count
    }

    pub fn add_stock(&mut self, amount: u32) -> Result<(), InventoryError> {
        if amount == 0 {
            return Err(InventoryError::ZeroChange);
        }

        self.count += amount;
        Ok(())
    }
}
```

Why this design matters:

- The fields are private, so outside code cannot create invalid states directly.
- `Item::new` validates input and returns `Result`.
- `InventoryError` names expected failure cases.
- `add_stock` mutates through `&mut self`, which makes mutation visible at the call site.

## Step 4: Add a Report Module

Create `src/report.rs`:

```rust
use crate::inventory::Item;

pub fn format_item_report(item: &Item) -> String {
    format!("{}: {} in stock", item.name(), item.count())
}
```

The report module does not need access to private fields. It uses the public
methods `name()` and `count()`. This keeps formatting separate from inventory
rules.

## Step 5: Keep main.rs Thin

Replace `src/main.rs`:

```rust
use inventory_cli::{format_item_report, Item};

fn main() {
    let mut item = Item::new("notebook", 3).expect("hard-coded item should be valid");
    item.add_stock(2).expect("hard-coded stock change should be valid");

    println!("{}", format_item_report(&item));
}
```

Run:

```bash
cargo run
```

Expected output:

```text
notebook: 5 in stock
```

`main.rs` should stay boring. It wires the application together. The business
rules live in the library where they can be tested and reused.

## Step 6: Test the Public API

Create `tests/inventory_flow.rs`:

```rust
use inventory_cli::{format_item_report, InventoryError, Item};

#[test]
fn creates_item_and_formats_report() {
    let mut item = Item::new("markers", 4).unwrap();
    item.add_stock(6).unwrap();

    assert_eq!(format_item_report(&item), "markers: 10 in stock");
}

#[test]
fn rejects_empty_name() {
    let error = Item::new("   ", 1).unwrap_err();

    assert_eq!(error, InventoryError::EmptyName);
}

#[test]
fn rejects_zero_stock_change() {
    let mut item = Item::new("paper", 10).unwrap();
    let error = item.add_stock(0).unwrap_err();

    assert_eq!(error, InventoryError::ZeroChange);
}
```

Run:

```bash
cargo test
```

These are integration tests. They use the crate the same way an external caller
would use it. That is why they are good for checking your public API design.

## Step 7: Add a Runnable Example

Create `examples/print_report.rs`:

```rust
use inventory_cli::{format_item_report, Item};

fn main() {
    let item = Item::new("coffee", 8).unwrap();

    println!("{}", format_item_report(&item));
}
```

Run:

```bash
cargo run --example print_report
```

Expected output:

```text
coffee: 8 in stock
```

Examples are useful for documentation, demos, and quick manual checks.

## Step 8: Run the Quality Loop

Run these before considering the project complete:

```bash
cargo fmt --check
cargo clippy -- -D warnings
cargo test
cargo run
cargo run --example print_report
```

If formatting fails, run:

```bash
cargo fmt
```

If Clippy fails, read the warning. Clippy is Rust's linter: it catches patterns
that compile but are suspicious, confusing, inefficient, or unidiomatic.

## Step 9: README Checklist

Create a small `README.md`:

````markdown
# Inventory CLI

A beginner-friendly Rust project that separates reusable inventory logic from a
thin command-line app.

## Commands

```bash
cargo run
cargo test
cargo run --example print_report
```

## Project Layout

- `src/lib.rs`: public library API
- `src/inventory.rs`: inventory data and validation rules
- `src/report.rs`: report formatting
- `src/main.rs`: command-line entry point
- `tests/`: integration tests
- `examples/`: runnable examples
````

## Extension Ideas

Build next:

- parse item name and count from command-line arguments
- store multiple items in a `Vec<Item>`
- load and save inventory from a text file
- add `remove_stock` with an error for removing too much
- implement `Display` for `InventoryError`
- publish generated documentation with `cargo doc --open`

## Completion Checklist

- `main.rs` is small and readable.
- public names are exported from `lib.rs`.
- struct fields are private unless external mutation is safe.
- invalid states return `Result`, not panic.
- integration tests cover success and failure.
- examples run with `cargo run --example`.
- `cargo fmt --check`, `cargo clippy -- -D warnings`, and `cargo test` pass.

---

[**Next ->** Rust Reference](../../reference/README.md)  
[**<- Previous** Modular Design and Codebase Cleanliness](./05-modular-design-and-codebase-cleanliness.md)
