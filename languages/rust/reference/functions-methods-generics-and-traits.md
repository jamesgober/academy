<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../README.md) / [Rust](../README.md) / [Reference](./README.md)

---

# Functions, Methods, Generics, and Traits Reference

> Lookup for function signatures, parameters, returns, methods, generic `<T>`, and trait bounds.

## Function Shapes

| Shape | Example | Use |
|---|---|---|
| no input, no return | `fn log_start() {}` | side effect only |
| input, no return | `fn print_name(name: &str) {}` | consume/read input and do work |
| input and return | `fn double(n: i32) -> i32 { n * 2 }` | compute value |
| fallible return | `fn parse(s: &str) -> Result<i32, ParseIntError>` | expected failure |
| optional return | `fn first(v: &[i32]) -> Option<&i32>` | value may not exist |

## Parameter Choices

| Parameter | Meaning | Use when |
|---|---|---|
| `value: T` | takes ownership or copies if `Copy` | function should own it |
| `value: &T` | shared borrow | read only |
| `value: &mut T` | mutable borrow | function changes caller's value |
| `value: impl Into<String>` | accepts convertible input | ergonomic constructors |
| `values: &[T]` | borrowed list view | read array or vector without owning |

```rust
fn total(values: &[u32]) -> u32 {
    values.iter().sum()
}
```

## Methods and Associated Functions

```rust
pub struct Item {
    name: String,
}

impl Item {
    pub fn new(name: impl Into<String>) -> Self {
        Self { name: name.into() }
    }

    pub fn name(&self) -> &str {
        &self.name
    }
}
```

| Receiver | Meaning |
|---|---|
| no `self` | associated function, called like `Item::new()` |
| `self` | consumes the value |
| `&self` | reads through shared borrow |
| `&mut self` | mutates through exclusive borrow |

## Generics

Generic `<T>` means "this code works with some type chosen by the caller or
compiler."

```rust
fn first<T>(values: &[T]) -> Option<&T> {
    values.first()
}
```

Use generics when the logic does not care about the exact type.

## Trait Bounds

A trait is a set of behavior a type can implement.

```rust
use std::fmt::Display;

fn print_label<T: Display>(label: T) {
    println!("{label}");
}
```

Equivalent `where` syntax:

```rust
use std::fmt::Display;

fn print_pair<T, U>(left: T, right: U)
where
    T: Display,
    U: Display,
{
    println!("{left}: {right}");
}
```

Use `where` when bounds become long.

## `impl Trait`

```rust
use std::fmt::Display;

fn print_label(label: impl Display) {
    println!("{label}");
}
```

`impl Trait` in parameters is concise. Named generics are better when multiple
parameters must be the same type.

```rust
fn same_type<T: PartialEq>(left: T, right: T) -> bool {
    left == right
}
```

## Risk Notes

| Pattern | Risk | Better habit |
|---|---|---|
| taking `String` when only reading | forces caller to give ownership | accept `&str` |
| returning `&T` from created local data | invalid lifetime | return owned `T` |
| too many generic parameters | hard to read errors | start concrete, generalize later |
| `unwrap()` in library functions | panic surprises caller | return `Result` or `Option` |

## Cross References

- [Types, Strings, and Collections](./types-strings-and-collections.md)
- [Ownership and Borrowing Cheat Sheet](./ownership-and-borrowing-cheat-sheet.md)
- [Modules, Visibility, and Exports](./modules-visibility-and-exports.md)
