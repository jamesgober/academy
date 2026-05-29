<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 02](./README.md)

---

# Borrowing and References

> Borrowing lets code use a value without becoming responsible for owning or dropping it.

**You will learn:**
- what `&T` and `&mut T` mean
- why shared reads and exclusive writes cannot overlap
- how dereferencing works
- how to narrow borrow scopes
- how to design functions that borrow instead of move
- how to fix common borrow-checker errors

**Before this page, you should know:** [Ownership: The One-Owner Rule](./02-ownership-the-one-owner-rule.md)

---

## Borrowing Without Owning

```rust
fn print_topic(topic: &str) {
    println!("topic: {topic}");
}

fn main() {
    let topic = String::from("borrowing");

    print_topic(&topic);
    println!("still owned by main: {topic}");
}
```

`&topic` creates a shared reference. A reference points at a value without taking
ownership.

```text
topic owns String
    |
    v
heap text
    ^
    |
borrowed &topic reads it temporarily
```

The owner is still `topic`.

## Prefer `&str` Over `&String`

This works:

```rust
fn print_name(name: &String) {
    println!("{name}");
}
```

But this is more flexible:

```rust
fn print_name(name: &str) {
    println!("{name}");
}
```

`&str` accepts string literals, `String`, and string slices.

```rust
let owned = String::from("Rust");

print_name("literal");
print_name(&owned);
print_name(&owned[0..2]);
```

Use `&String` only when you specifically need `String` methods that are not
available on `str`, which is uncommon.

## Mutable Borrowing

A mutable borrow gives temporary exclusive write access.

```rust
fn add_minutes(minutes: &mut u32, extra: u32) {
    *minutes += extra;
}

fn main() {
    let mut minutes = 30;
    add_minutes(&mut minutes, 15);

    println!("{minutes}");
}
```

`*minutes` dereferences the reference. Dereference means "go to the value this
reference points to."

## Why Mutable Borrows Are Exclusive

At one time, Rust allows either:

- many shared references, or
- one mutable reference

Not both.

```text
Safe:
value <- &T
value <- &T
value <- &T

Safe:
value <- &mut T

Not safe:
value <- &T
value <- &mut T at same time
```

This prevents code from reading a value while another part of the code changes
it behind its back.

## Borrow Scope

Borrow scope is how long the borrow is used.

```rust
fn main() {
    let mut topic = String::from("Rust");

    let first = &topic;
    println!("{first}");

    let second = &mut topic;
    second.push_str(" borrowing");

    println!("{topic}");
}
```

This works because the shared borrow `first` is no longer used after its
`println!`. Modern Rust can often see that the borrow ends before the variable's
textual scope ends.

If a borrow lasts too long, add a block:

```rust
fn main() {
    let mut topic = String::from("Rust");

    {
        let first = &topic;
        println!("{first}");
    }

    let second = &mut topic;
    second.push_str(" borrowing");
}
```

## Common Error: Two Mutable Borrows

```rust
fn main() {
    let mut topic = String::from("Rust");

    let a = &mut topic;
    let b = &mut topic;

    println!("{a} {b}");
}
```

Rust reports something like:

```text
error[E0499]: cannot borrow `topic` as mutable more than once at a time
```

Beginner translation: "Two parts of the program are trying to have exclusive
write access at the same time."

Fix by sequencing the work:

```rust
fn main() {
    let mut topic = String::from("Rust");

    {
        let a = &mut topic;
        a.push_str(" ownership");
    }

    {
        let b = &mut topic;
        b.push_str(" borrowing");
    }

    println!("{topic}");
}
```

## Borrowing Struct Fields

Rust can borrow different fields separately in many cases.

```rust
struct StudyEntry {
    topic: String,
    minutes: u32,
}

fn main() {
    let mut entry = StudyEntry {
        topic: String::from("Rust"),
        minutes: 30,
    };

    let topic = &entry.topic;
    let minutes = &mut entry.minutes;

    *minutes += 15;
    println!("{topic}: {minutes}");
}
```

This works because `topic` and `minutes` are different fields.

## Borrowing in Real API Design

```rust
#[derive(Debug)]
struct StudyEntry {
    topic: String,
    minutes: u32,
}

fn print_entry(entry: &StudyEntry) {
    println!("{}: {} minutes", entry.topic, entry.minutes);
}

fn add_time(entry: &mut StudyEntry, extra: u32) {
    entry.minutes += extra;
}

fn main() {
    let mut entry = StudyEntry {
        topic: String::from("Rust"),
        minutes: 30,
    };

    print_entry(&entry);
    add_time(&mut entry, 15);
    print_entry(&entry);
}
```

The function signatures tell the story:

- `print_entry(&StudyEntry)` reads only
- `add_time(&mut StudyEntry)` mutates

## Decision Table

| Function need | Parameter |
|---|---|
| read text | `&str` |
| read struct | `&StudyEntry` |
| mutate struct | `&mut StudyEntry` |
| read list | `&[StudyEntry]` |
| store value | owned `StudyEntry` or `String` |

## Reference Links

- [Ownership and Borrowing Cheat Sheet](../../reference/ownership-and-borrowing-cheat-sheet.md)
- [Functions, Methods, Generics, and Traits](../../reference/functions-methods-generics-and-traits.md)
- [Errors, Warnings, and Debugging](../../reference/errors-warnings-and-debugging.md)

---

## Recap

- `&T` gives shared read access.
- `&mut T` gives exclusive write access.
- Shared and mutable borrows cannot overlap freely.
- Dereferencing with `*` accesses the referenced value.
- Narrow borrow scopes when the compiler says a borrow lasts too long.
- Borrowing is the normal fix when ownership transfer is unnecessary.

## Try It Yourself

Create a `StudyEntry` and write:

- `print_entry(entry: &StudyEntry)`
- `add_minutes(entry: &mut StudyEntry, extra: u32)`
- `topic(entry: &StudyEntry) -> &str`

Call them in a valid order and explain why the borrows do not conflict.

---

[**Next ->** Lifetimes in Plain Language](./04-lifetimes-in-plain-language.md)  
[**<- Previous** Ownership: The One-Owner Rule](./02-ownership-the-one-owner-rule.md)
