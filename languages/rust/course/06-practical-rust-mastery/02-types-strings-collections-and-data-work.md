<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 06](./README.md)

---

# Types, Strings, Collections, and Data Work

> Useful Rust programs spend a lot of time cleaning text, storing lists, looking things up, and transforming data.

**You will learn:**
- numeric and boolean type choices
- `String` versus `&str` in real APIs
- arrays, slices, `Vec<T>`, `HashMap<K, V>`, and `HashSet<T>`
- safe indexing and absence handling
- iterator-based data transformations
- how to build a small report from structured data

**Before this page, you should know:** [Functions, Parameters, Ownership, and Return Design](./01-functions-parameters-ownership-and-return-design.md)

---

## Choose Types by Meaning

Rust has many numeric types because systems code sometimes cares about exact
size. Beginners should start with meaning:

| Need | Good default |
|---|---|
| normal signed whole number | `i32` |
| count that cannot be negative | `u32` |
| indexing/sizes | `usize` |
| large whole number | `i64` or `u64` |
| normal decimal number | `f64` |
| true/false | `bool` |
| one Unicode scalar value | `char` |

Do not use `usize` for every positive number. Use it for sizes and indexes.

## `String` Versus `&str`

`String` owns growable UTF-8 text. `&str` borrows UTF-8 text.

```rust
let owned: String = String::from("Rust");
let borrowed: &str = "Rust";
let view: &str = owned.as_str();
```

Use this rule:

- accept `&str` when reading text
- store `String` inside structs
- return `String` when creating new text

```rust
fn format_minutes(topic: &str, minutes: u32) -> String {
    format!("{topic}: {minutes} minutes")
}
```

`format!` creates a new `String`. It is perfect for building display text.

## String Operations You Actually Use

```rust
let raw = "  Rust ownership  ";
let clean = raw.trim();

assert_eq!(clean, "Rust ownership");
assert!(clean.contains("ownership"));
assert!(clean.starts_with("Rust"));
```

Splitting:

```rust
let line = "Rust,45";
let parts: Vec<&str> = line.split(',').collect();

assert_eq!(parts, vec!["Rust", "45"]);
```

Parsing:

```rust
let minutes: u32 = "45".parse()?;
```

The `?` works when your function returns a compatible `Result`.

## Arrays and Slices

An array has fixed length:

```rust
let scores: [u32; 3] = [10, 20, 30];
```

A slice is a borrowed view into a list:

```rust
fn total(values: &[u32]) -> u32 {
    values.iter().sum()
}

let scores = [10, 20, 30];
let total_score = total(&scores);
```

Accept slices when a function only needs to read a list. That lets callers pass
arrays, vectors, or part of a vector.

## `Vec<T>`: Growable Lists

```rust
let mut entries = Vec::new();
entries.push("Rust");
entries.push("Cargo");

let last = entries.pop();
```

Common vector methods:

| Method | Returns | Use |
|---|---|---|
| `push(value)` | `()` | add to end |
| `pop()` | `Option<T>` | remove from end |
| `get(index)` | `Option<&T>` | safe index |
| `len()` | `usize` | item count |
| `is_empty()` | `bool` | emptiness check |
| `iter()` | iterator of `&T` | read items |
| `iter_mut()` | iterator of `&mut T` | mutate items |
| `into_iter()` | iterator of `T` | consume vector |

Prefer `get` when an index may be out of range:

```rust
if let Some(first) = entries.get(0) {
    println!("first topic: {first}");
}
```

Indexing with `entries[0]` panics if the index is missing.

## `HashMap<K, V>`: Lookup by Key

```rust
use std::collections::HashMap;

let mut minutes_by_topic = HashMap::new();
minutes_by_topic.insert(String::from("Rust"), 45);
minutes_by_topic.insert(String::from("Cargo"), 20);
```

Read safely:

```rust
if let Some(minutes) = minutes_by_topic.get("Rust") {
    println!("Rust minutes: {minutes}");
}
```

Count grouped data:

```rust
use std::collections::HashMap;

fn totals_by_topic(entries: &[StudyEntry]) -> HashMap<String, u32> {
    let mut totals = HashMap::new();

    for entry in entries {
        let total = totals.entry(entry.topic().to_owned()).or_insert(0);
        *total += entry.minutes();
    }

    totals
}
```

`entry(...).or_insert(0)` means "get this key's value, or insert zero first."

## `HashSet<T>`: Unique Values

```rust
use std::collections::HashSet;

let mut topics = HashSet::new();
topics.insert("Rust");
topics.insert("Rust");
topics.insert("Cargo");

assert_eq!(topics.len(), 2);
```

Use sets for uniqueness and membership checks.

## Real Data Model

Use the `StudyEntry` from the previous lesson:

```rust
#[derive(Debug, Clone, PartialEq, Eq)]
pub struct StudyEntry {
    topic: String,
    minutes: u32,
}

impl StudyEntry {
    pub fn new(topic: impl Into<String>, minutes: u32) -> Result<Self, EntryError> {
        let topic = normalize_topic(&topic.into()).ok_or(EntryError::EmptyTopic)?;

        if minutes == 0 || minutes > 24 * 60 {
            return Err(EntryError::InvalidMinutes);
        }

        Ok(Self { topic, minutes })
    }

    pub fn topic(&self) -> &str {
        &self.topic
    }

    pub fn minutes(&self) -> u32 {
        self.minutes
    }
}
```

This repeats the constructor pattern from lesson 01 so the following collection
examples have a complete type to work with.

## Build a Report

```rust
use std::collections::HashMap;

pub fn total_minutes(entries: &[StudyEntry]) -> u32 {
    entries.iter().map(StudyEntry::minutes).sum()
}

pub fn topics(entries: &[StudyEntry]) -> Vec<String> {
    let mut names = entries
        .iter()
        .map(|entry| entry.topic().to_owned())
        .collect::<Vec<_>>();

    names.sort();
    names.dedup();
    names
}

pub fn totals_by_topic(entries: &[StudyEntry]) -> HashMap<String, u32> {
    let mut totals = HashMap::new();

    for entry in entries {
        *totals.entry(entry.topic().to_owned()).or_insert(0) += entry.minutes();
    }

    totals
}
```

This is real data work:

- `iter()` borrows entries.
- `map(...)` transforms entries into minutes or topic names.
- `sum()` adds numeric iterator items.
- `collect::<Vec<_>>()` builds a vector.
- `sort()` and `dedup()` clean up topic names.
- `HashMap` groups totals by topic.

## Display the Report

```rust
pub fn format_report(entries: &[StudyEntry]) -> String {
    let total = total_minutes(entries);
    let mut lines = vec![format!("Total study time: {total} minutes")];

    let mut by_topic = totals_by_topic(entries)
        .into_iter()
        .collect::<Vec<_>>();

    by_topic.sort_by(|a, b| a.0.cmp(&b.0));

    for (topic, minutes) in by_topic {
        lines.push(format!("- {topic}: {minutes} minutes"));
    }

    lines.join("\n")
}
```

This function returns a `String` because it creates new display text.

## Reference Links

- [Types, Strings, and Collections](../../reference/types-strings-and-collections.md)
- [Functions, Methods, Generics, and Traits](../../reference/functions-methods-generics-and-traits.md)

---

## Recap

- `String` owns text; `&str` borrows text.
- `Vec<T>` is the normal growable list.
- `HashMap<K, V>` is for key/value lookup and grouping.
- `HashSet<T>` is for uniqueness.
- Prefer slices like `&[T]` for read-only list parameters.
- Real Rust data work often combines iterators, collections, and explicit absence handling.

## Try It Yourself

Add `average_minutes(entries: &[StudyEntry]) -> Option<f64>`. It should return
`None` for an empty slice and `Some(average)` otherwise. Then add a test for both
cases.

---

[**Next ->** Traits, Generics, Iterators, and Closures](./03-traits-generics-iterators-and-closures.md)  
[**<- Previous** Functions, Parameters, Ownership, and Return Design](./01-functions-parameters-ownership-and-return-design.md)
