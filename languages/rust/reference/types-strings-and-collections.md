<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../README.md) / [Rust](../README.md) / [Reference](./README.md)

---

# Types, Strings, and Collections Reference

> Lookup for common Rust data types, string choices, arrays, slices, vectors, and maps.

## Scalar Types

| Type | Description | Example | Notice |
|---|---|---|---|
| `bool` | true/false | `let ok = true;` | Conditions require `bool`. |
| `char` | one Unicode scalar value | `let letter = 'R';` | Not the same as one byte. |
| `i32` | signed 32-bit integer | `let n: i32 = -5;` | Default integer type. |
| `u32` | unsigned 32-bit integer | `let n: u32 = 5;` | Cannot represent negative values. |
| `usize` | pointer-sized unsigned integer | `let i: usize = 0;` | Used for indexing. |
| `f64` | 64-bit float | `let x = 3.14;` | Default float type. |

Common integer ranges:

| Type | Min | Max |
|---|---:|---:|
| `i8` | -128 | 127 |
| `u8` | 0 | 255 |
| `i32` | -2147483648 | 2147483647 |
| `u32` | 0 | 4294967295 |
| `i64` | -9223372036854775808 | 9223372036854775807 |
| `u64` | 0 | 18446744073709551615 |

## `&str` Versus `String`

| Type | Owns text? | Growable? | Typical use |
|---|---:|---:|---|
| `&str` | no | no | borrowed text, string literals, function parameters |
| `String` | yes | yes | building or storing owned text |

```rust
fn greet(name: &str) -> String {
    format!("Hello, {name}")
}

let literal: &str = "James";
let owned: String = String::from("Rust");
```

Prefer `&str` for function parameters when the function only needs to read text.
Return `String` when the function creates new owned text.

## Arrays and Slices

Array: fixed-size collection stored inline.

```rust
let scores: [i32; 3] = [10, 20, 30];
```

Slice: borrowed view into a sequence.

```rust
let first_two: &[i32] = &scores[0..2];
```

| Syntax | Meaning |
|---|---|
| `[T; N]` | array of `N` values |
| `&[T]` | borrowed slice |
| `&values[..]` | slice of all values |
| `&values[start..end]` | range slice, end excluded |

Risk: slicing with invalid byte boundaries in strings or invalid indexes in
arrays can panic. Prefer iterator methods when possible.

## `Vec<T>`

`Vec<T>` is a growable list.

```rust
let mut names = Vec::new();
names.push(String::from("Ada"));
names.push(String::from("Grace"));

let last = names.pop();
let first = &names[0];
let maybe_first = names.get(0);
```

| Method | Parameters | Returns | Notice |
|---|---|---|---|
| `push(value)` | `T` | `()` | adds to end |
| `pop()` | none | `Option<T>` | removes and returns last item |
| `get(index)` | `usize` | `Option<&T>` | safe indexing |
| `len()` | none | `usize` | item count |
| `is_empty()` | none | `bool` | clearer than `len() == 0` |
| `iter()` | none | iterator of `&T` | read without moving |
| `iter_mut()` | none | iterator of `&mut T` | mutate items |
| `into_iter()` | self | iterator of `T` | consumes vector |
| `sort()` | none | `()` | requires sortable items |
| `retain(predicate)` | closure | `()` | keeps matching items |

## `HashMap<K, V>`

```rust
use std::collections::HashMap;

let mut counts = HashMap::new();
counts.insert("notebook", 3);
counts.insert("pen", 10);

let notebooks = counts.get("notebook");
```

| Method | Use |
|---|---|
| `insert(key, value)` | add or replace |
| `get(key)` | read optional value |
| `entry(key).or_insert(value)` | insert default if missing |
| `remove(key)` | remove by key |
| `contains_key(key)` | check existence |

## Cross References

- [Ownership and Borrowing Cheat Sheet](./ownership-and-borrowing-cheat-sheet.md)
- [Functions, Methods, Generics, and Traits](./functions-methods-generics-and-traits.md)
- [Errors, Warnings, and Debugging](./errors-warnings-and-debugging.md)
