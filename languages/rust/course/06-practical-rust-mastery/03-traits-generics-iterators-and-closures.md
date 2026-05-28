<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 06](./README.md)

---

# Traits, Generics, Iterators, and Closures

> Rust gets expressive when you can write behavior once and let types prove what is allowed.

**You will learn:**
- what generics like `<T>` mean
- how traits describe shared behavior
- how trait bounds make generic code safe
- how iterators and closures replace many manual loops
- when to keep code concrete instead of over-abstracting

**Before this page, you should know:** [Types, Strings, Collections, and Data Work](./02-types-strings-collections-and-data-work.md)

---

## Generics Without the Fog

Generic code uses a placeholder type.

```rust
fn first<T>(values: &[T]) -> Option<&T> {
    values.first()
}
```

`T` means "some type." The caller decides the concrete type:

```rust
let numbers = vec![1, 2, 3];
let names = vec!["Ada", "Grace"];

assert_eq!(first(&numbers), Some(&1));
assert_eq!(first(&names), Some(&"Ada"));
```

Use generics when the logic truly does not care about the exact type.

## Traits Describe Behavior

A trait is a named set of behavior a type can implement.

```rust
pub trait Summary {
    fn summarize(&self) -> String;
}
```

Implement it:

```rust
impl Summary for StudyEntry {
    fn summarize(&self) -> String {
        format!("{} for {} minutes", self.topic(), self.minutes())
    }
}
```

Call it:

```rust
fn print_summary(item: &impl Summary) {
    println!("{}", item.summarize());
}
```

Plain English: "This function accepts a borrowed value of any type that knows
how to summarize itself."

## Trait Bounds

Equivalent generic form:

```rust
fn print_summary<T: Summary>(item: &T) {
    println!("{}", item.summarize());
}
```

Use `where` when bounds become longer:

```rust
fn print_twice<T>(item: &T)
where
    T: Summary,
{
    println!("{}", item.summarize());
    println!("{}", item.summarize());
}
```

## Common Standard Traits

| Trait | Means | Often derived? |
|---|---|---:|
| `Debug` | can be formatted with `{:?}` | yes |
| `Clone` | can make an explicit copy | yes |
| `Copy` | copied by assignment, no move | sometimes |
| `PartialEq` | can compare with `==` | yes |
| `Eq` | equality is total | yes |
| `PartialOrd` | can compare order sometimes | yes |
| `Ord` | total ordering | yes |
| `Default` | has a default value | sometimes |
| `Display` | user-facing formatting | no, implement manually |

Derive when the automatic behavior is correct:

```rust
#[derive(Debug, Clone, PartialEq, Eq)]
pub struct StudyEntry {
    topic: String,
    minutes: u32,
}
```

Implement manually when you need a custom result:

```rust
use std::fmt;

impl fmt::Display for StudyEntry {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "{} ({} minutes)", self.topic(), self.minutes())
    }
}
```

## Iterators

An iterator produces values one at a time. Many collection methods return
iterators.

```rust
let minutes = entries.iter().map(StudyEntry::minutes);
```

Iterator chains are lazy until consumed by methods like `collect`, `sum`, or a
`for` loop.

```rust
let total: u32 = entries
    .iter()
    .map(StudyEntry::minutes)
    .sum();
```

Common iterator methods:

| Method | Use |
|---|---|
| `map` | transform each item |
| `filter` | keep matching items |
| `find` | find first match |
| `any` | at least one match |
| `all` | every item matches |
| `fold` | build one value |
| `collect` | build a collection |
| `enumerate` | add index |
| `take` | limit count |
| `skip` | ignore first items |

## Closures

A closure is an inline function value.

```rust
let long_sessions = entries
    .iter()
    .filter(|entry| entry.minutes() >= 60)
    .collect::<Vec<_>>();
```

`|entry| entry.minutes() >= 60` is a closure.

Closures can capture variables:

```rust
let minimum = 30;

let focused = entries
    .iter()
    .filter(|entry| entry.minutes() >= minimum)
    .collect::<Vec<_>>();
```

The closure borrows `minimum` from the surrounding scope.

## Real Generic Helper

```rust
pub fn find_by_topic<'a, T>(items: &'a [T], topic: &str) -> Option<&'a T>
where
    T: HasTopic,
{
    items.iter().find(|item| item.topic() == topic)
}

pub trait HasTopic {
    fn topic(&self) -> &str;
}

impl HasTopic for StudyEntry {
    fn topic(&self) -> &str {
        self.topic()
    }
}
```

This is intentionally more advanced. The trait says "anything with a topic can
be searched by topic." The function works for any slice of those items.

Do not start every app with generic helpers. Start concrete. Generalize after
two or three real use cases prove the pattern.

## `impl Trait` Return Values

You can return an iterator without naming the exact iterator type:

```rust
pub fn passing_entries(entries: &[StudyEntry]) -> impl Iterator<Item = &StudyEntry> {
    entries.iter().filter(|entry| entry.minutes() >= 30)
}
```

This says, "I return some iterator that yields borrowed `StudyEntry` values."

## Reference Links

- [Functions, Methods, Generics, and Traits](../../reference/functions-methods-generics-and-traits.md)
- [Types, Strings, and Collections](../../reference/types-strings-and-collections.md)

---

## Recap

- Generics let code work over multiple types.
- Traits define shared behavior.
- Trait bounds tell Rust what generic code is allowed to do.
- Iterators transform collections without manual indexing.
- Closures are inline function values and can capture surrounding variables.
- Start concrete, then generalize when a real pattern appears.

## Try It Yourself

Create a `Summary` trait for `StudyEntry`, implement `Display`, then write
`print_all<T: Summary>(items: &[T])`. After that, rewrite it with a `where`
clause.

---

[**Next ->** Files, CLI Input, Environment, and Useful Programs](./04-files-cli-input-environment-and-useful-programs.md)  
[**<- Previous** Types, Strings, Collections, and Data Work](./02-types-strings-collections-and-data-work.md)
