<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 02](./README.md)

---

# Ownership: The One-Owner Rule

> Ownership is Rust's way of knowing exactly who is responsible for cleaning up a value.

**You will learn:**
- what an owner is
- why some assignments copy and others move
- what stack and heap mean at beginner level
- how ownership moves through functions
- when to borrow, clone, or return ownership
- how to read use-after-move compiler errors

**Before this page, you should know:** [Variables, Mutability, and Shadowing](./01-variables-mutability-and-shadowing.md)

---

## Why Ownership Exists

Programs create values. Some values need cleanup. For example, a `String` stores
text on the heap, which is memory managed while the program runs.

Many languages use a garbage collector to clean memory later. Rust does not use
a garbage collector for normal ownership. Instead, Rust knows at compile time
which variable owns each value. When the owner goes out of scope, Rust drops the
value.

Ownership gives Rust memory safety without a garbage collector.

## The Three Rules

1. Each value has one owner.
2. Ownership can move to another owner.
3. When the owner goes out of scope, the value is dropped.

Do not memorize these like a slogan. Use them to answer one practical question:

```text
Who is responsible for this value right now?
```

## Stack and Heap: Simple Version

The stack stores simple, fixed-size values and function call data. The heap
stores values that need flexible size or longer storage, such as `String` data.

```text
stack:
car_name variable
    |
    v
heap:
"Comet" text bytes
```

The `String` value itself has stack metadata, but it owns bytes on the heap.
That ownership matters.

## Copy Types

Small fixed-size values usually implement `Copy`.

```rust
fn main() {
    let minutes = 45;
    let copied_minutes = minutes;

    println!("{minutes}");
    println!("{copied_minutes}");
}
```

`i32`, `u32`, `bool`, `char`, and many simple numeric types copy because copying
them is cheap and safe.

## Move Types

`String` does not copy by default.

```rust
fn main() {
    let topic = String::from("ownership");
    let moved_topic = topic;

    // println!("{topic}"); // error: value borrowed after move
    println!("{moved_topic}");
}
```

After `let moved_topic = topic;`, the new owner is `moved_topic`.

```text
Before move:
topic -------> heap text "ownership"

After move:
moved_topic -> heap text "ownership"
topic ----X   no longer valid
```

Rust prevents both variables from thinking they own the same heap allocation.
Otherwise both might try to free the same memory.

## Reading the Error

If you try to use `topic` after the move, Rust reports something like:

```text
error[E0382]: borrow of moved value: `topic`
```

Beginner translation:

"You transferred ownership away from `topic`, then tried to use `topic` again."

Common fixes:

- use the new owner
- borrow instead of moving
- clone if you truly need two owned copies
- return ownership from a function

## Ownership and Functions

Passing an owned `String` to this function moves it:

```rust
fn print_owned(topic: String) {
    println!("{topic}");
}

fn main() {
    let topic = String::from("Rust");
    print_owned(topic);
    // println!("{topic}"); // moved into print_owned
}
```

If the function only needs to read, borrow instead:

```rust
fn print_borrowed(topic: &str) {
    println!("{topic}");
}

fn main() {
    let topic = String::from("Rust");
    print_borrowed(&topic);
    println!("{topic}");
}
```

This is one of the biggest Rust design habits: do not take ownership unless the
function needs ownership.

## Return Ownership

Sometimes a function should take ownership, transform the value, and return it.

```rust
fn add_suffix(mut topic: String) -> String {
    topic.push_str(" notes");
    topic
}

fn main() {
    let topic = String::from("Rust");
    let topic = add_suffix(topic);

    println!("{topic}");
}
```

The value moves into `add_suffix`, then moves back out as the return value.

## Clone Intentionally

`clone()` creates a real duplicate.

```rust
fn main() {
    let topic = String::from("Rust");
    let copy = topic.clone();

    println!("{topic}");
    println!("{copy}");
}
```

Cloning is not evil. Cloning is a cost. Use it when you truly need another owned
value.

Bad reason to clone:

```text
I do not understand why the borrow checker is upset.
```

Good reason to clone:

```text
This value must be stored independently in two places.
```

## Real Example: Build Entries Without Cloning Everything

```rust
#[derive(Debug)]
struct StudyEntry {
    topic: String,
    minutes: u32,
}

fn create_entry(topic: String, minutes: u32) -> StudyEntry {
    StudyEntry { topic, minutes }
}

fn print_entry(entry: &StudyEntry) {
    println!("{} for {} minutes", entry.topic, entry.minutes);
}

fn main() {
    let topic = String::from("ownership");
    let entry = create_entry(topic, 45);

    print_entry(&entry);
    print_entry(&entry);
}
```

`create_entry` takes ownership because the struct stores the `String`.
`print_entry` borrows because it only reads.

That design avoids unnecessary cloning and keeps ownership clear.

## Decision Table

| Situation | Prefer |
|---|---|
| function only reads text | `&str` |
| function only reads a struct | `&StructName` |
| function changes caller's value | `&mut StructName` |
| function stores value for later | owned `String`/`T` |
| function creates new value | return owned value |
| caller needs independent copy | `clone()` |

## Reference Links

- [Ownership and Borrowing Cheat Sheet](../../reference/ownership-and-borrowing-cheat-sheet.md)
- [Functions, Methods, Generics, and Traits](../../reference/functions-methods-generics-and-traits.md)

---

## Recap

- Ownership answers who cleans up a value.
- Copy types duplicate cheaply; move types transfer ownership.
- Passing owned values to functions can move them.
- Borrow when a function only needs to read.
- Clone intentionally, not as a reflex.

## Try It Yourself

Write a `StudyEntry` struct and three functions:

- `create_entry(topic: String, minutes: u32) -> StudyEntry`
- `print_entry(entry: &StudyEntry)`
- `rename_entry(entry: &mut StudyEntry, topic: String)`

Explain which function owns, borrows, and mutably borrows.

---

[**Next ->** Borrowing and References](./03-borrowing-and-references.md)  
[**<- Previous** Variables, Mutability, and Shadowing](./01-variables-mutability-and-shadowing.md)
