<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 02](./README.md)

---

# Lifetimes in Plain Language

> Lifetimes are Rust's way of proving that references do not point to dead values.

**You will learn:**
- what lifetimes mean without academic fog
- why most lifetimes are inferred
- when annotations are needed
- how to read lifetime-heavy signatures
- when returning owned data is simpler than returning references

**Before this page, you should know:** [Borrowing and References](./03-borrowing-and-references.md)

---

## The Problem Lifetimes Solve

A reference must never outlive the value it points to.

Bad idea:

```rust
fn broken() -> &str {
    let topic = String::from("Rust");
    &topic
}
```

This cannot compile. `topic` is dropped when `broken` ends, so returning a
reference to it would create a dangling reference.

Beginner translation:

```text
topic exists only inside broken()
returned reference would escape broken()
reference would point to deleted data
Rust says no
```

Return owned data instead:

```rust
fn fixed() -> String {
    let topic = String::from("Rust");
    topic
}
```

## Lifetime Visual Model

```text
owner:      [ value exists ---------------- ]
reference:       [ reference is used ]

valid: reference ends before owner ends
```

Invalid:

```text
owner:      [ value exists ---- ]
reference:       [ reference keeps going -------- ]

invalid: reference outlives owner
```

## Most Lifetimes Are Inferred

You usually write:

```rust
fn first(entries: &[String]) -> Option<&String> {
    entries.first()
}
```

Rust understands the returned reference comes from `entries`.

The explicit idea is:

```rust
fn first<'a>(entries: &'a [String]) -> Option<&'a String> {
    entries.first()
}
```

`'a` is a name for a lifetime relationship. It does not create a runtime timer.
It tells the compiler, "the returned reference is tied to the input slice."

## When Annotations Are Needed

Rust needs help when a function returns a reference and there is more than one
input reference it could come from.

```rust
fn longer<'a>(left: &'a str, right: &'a str) -> &'a str {
    if left.len() >= right.len() {
        left
    } else {
        right
    }
}
```

Plain English:

"The returned reference will be valid only as long as both input references are
valid."

Why both? Because the function may return either one.

## Lifetime Names Are Just Names

`'a` is conventional, not magic.

```rust
fn longer<'input>(left: &'input str, right: &'input str) -> &'input str {
    if left.len() >= right.len() { left } else { right }
}
```

Use short names in normal code. Use clearer names if they genuinely help.

## Structs Holding References

A struct can hold a reference, but then the struct needs a lifetime parameter.

```rust
struct TopicView<'a> {
    topic: &'a str,
}

fn main() {
    let topic = String::from("Rust");
    let view = TopicView { topic: &topic };

    println!("{}", view.topic);
}
```

Plain English:

"`TopicView` cannot live longer than the text it points to."

For beginners, owned structs are often easier:

```rust
struct Topic {
    topic: String,
}
```

Use reference-holding structs when you need views into existing data and you
understand the ownership relationship.

## `'static`

`'static` means a reference can live for the entire program.

String literals are `'static`:

```rust
let name: &'static str = "Rust";
```

Do not slap `'static` onto errors to silence the compiler. If Rust says a value
does not live long enough, the fix is usually ownership design, not forcing
`'static`.

## Lifetime Error Triage

When you hit a lifetime error, ask:

1. What value owns the data?
2. Where is that owner dropped?
3. Where is the reference used?
4. Is the reference trying to escape the owner?
5. Would returning owned data be simpler?

Example fix:

```rust
fn label(topic: &str) -> String {
    format!("Topic: {topic}")
}
```

Returning `String` avoids tying output to borrowed input.

## Real Example: Find an Entry

```rust
#[derive(Debug)]
struct StudyEntry {
    topic: String,
    minutes: u32,
}

fn find_by_topic<'a>(entries: &'a [StudyEntry], topic: &str) -> Option<&'a StudyEntry> {
    entries.iter().find(|entry| entry.topic == topic)
}
```

The returned entry reference must come from `entries`, not from `topic`.

Rust can infer this:

```rust
fn find_by_topic(entries: &[StudyEntry], topic: &str) -> Option<&StudyEntry> {
    entries.iter().find(|entry| entry.topic == topic)
}
```

The explicit version is useful for learning.

## Reference Links

- [Ownership and Borrowing Cheat Sheet](../../reference/ownership-and-borrowing-cheat-sheet.md)
- [Functions, Methods, Generics, and Traits](../../reference/functions-methods-generics-and-traits.md)
- [Errors, Warnings, and Debugging](../../reference/errors-warnings-and-debugging.md)

---

## Recap

- Lifetimes prove references stay valid.
- Most lifetimes are inferred.
- Annotations describe relationships between input and output references.
- Returning owned data is often simpler for beginners.
- Structs with references need lifetime parameters.
- `'static` is not a magic fix.

## Try It Yourself

Write `find_long_session(entries: &[StudyEntry]) -> Option<&StudyEntry>` that
returns the first entry with at least 60 minutes. Then write the same signature
with explicit lifetime annotations.

---

[**Next ->** Structs, Enums, and Pattern Matching](./05-structs-enums-and-pattern-matching.md)  
[**<- Previous** Borrowing and References](./03-borrowing-and-references.md)
