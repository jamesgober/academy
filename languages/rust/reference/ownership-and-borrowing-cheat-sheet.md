<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../README.md) / [Rust](../README.md) / [Reference](./README.md)

---

# Ownership And Borrowing Reference

> Fast lookup for Rust's ownership, borrowing, move, clone, reference, and
> lifetime rules.

Full lessons:

- [Ownership: The One-Owner Rule](../course/02-rust-core-mental-model/02-ownership-the-one-owner-rule.md)
- [Borrowing And References](../course/02-rust-core-mental-model/03-borrowing-and-references.md)
- [Lifetimes In Plain Language](../course/02-rust-core-mental-model/04-lifetimes-in-plain-language.md)

---

## Ownership Rules

| Rule | Meaning |
|---|---|
| Each value has one owner | One variable is responsible for cleanup |
| Move transfers ownership | Old owner cannot use moved value |
| Drop runs when owner goes out of scope | Resources are cleaned automatically |
| `Copy` values duplicate cheaply | Old value remains usable |
| `Clone` duplicates explicitly | You asked for a new owned value |

Move:

```rust
let first = String::from("Rust");
let second = first;

// println!("{first}"); // error: moved
println!("{second}");
```

Clone:

```rust
let first = String::from("Rust");
let second = first.clone();

println!("{first}");
println!("{second}");
```

---

## Parameter Choices

| Signature | Means | Use when |
|---|---|---|
| `fn f(value: String)` | Takes ownership | Function stores or consumes it |
| `fn f(value: &String)` | Borrows a `String` | Rare; prefer `&str` for text |
| `fn f(value: &str)` | Borrows string-like text | Function only reads text |
| `fn f(values: Vec<T>)` | Takes ownership of vector | Function consumes or returns it |
| `fn f(values: &[T])` | Borrows list-like data | Function only reads list |
| `fn f(values: &mut Vec<T>)` | Mutably borrows vector | Function edits caller's vector |

Good beginner default:

```rust
fn word_count(text: &str) -> usize {
    text.split_whitespace().count()
}
```

This accepts `&String`, string literals, and string slices.

---

## Borrowing Rules

At one time, you can have:

```text
many shared references
```

or:

```text
one mutable reference
```

But not both.

Allowed:

```rust
let name = String::from("Rust");
let a = &name;
let b = &name;

println!("{a} {b}");
```

Allowed:

```rust
let mut name = String::from("Rust");
let editable = &mut name;

editable.push('!');
```

Rejected:

```rust
let mut name = String::from("Rust");
let read = &name;
let write = &mut name;

println!("{read} {write}");
```

---

## Common Fixes

| Error shape | Likely issue | Fix |
|---|---|---|
| value borrowed after move | Ownership moved earlier | Borrow instead, return value, or clone intentionally |
| cannot borrow as mutable | Binding or reference is not mutable | Add `mut` where ownership allows |
| cannot borrow mutable more than once | Overlapping `&mut` borrows | Shorten scopes or split operations |
| borrowed value does not live long enough | Reference outlives owner | Return owned value or tie lifetime to input |
| cannot move out of borrowed content | You tried to take ownership through a borrow | Clone, replace, or redesign ownership |

Shorten a borrow:

```rust
let mut items = vec!["a".to_string()];

{
    let first = &items[0];
    println!("{first}");
}

items.push("b".to_string());
```

---

## Lifetime Translation

Lifetime annotations do not make values live longer.

They describe relationships between references.

```rust
fn longest<'a>(left: &'a str, right: &'a str) -> &'a str {
    if left.len() >= right.len() {
        left
    } else {
        right
    }
}
```

Plain meaning:

```text
The returned reference is valid only as long as both input references are valid.
```

If that feels restrictive, return an owned `String` instead.

---

## Decision Model

Ask:

```text
Does this function need to keep the value?
  yes -> take ownership
  no  -> borrow

Does this function need to modify caller's value?
  yes -> &mut T
  no  -> &T, &str, or &[T]

Is cloning cheap and clearer?
  yes -> clone may be fine
  no  -> redesign ownership
```

---

## Risk Notices

| Pattern | Notice |
|---|---|
| Sprinkling `.clone()` everywhere | May hide ownership design problems |
| Returning references from functions | Requires owner to live long enough |
| Using `&String` in APIs | Usually less flexible than `&str` |
| Holding mutable borrows too long | Blocks other useful access |
| Fighting the borrow checker blindly | Step back and ask who owns the data |

---

[Reference Index](./README.md) / [Rust](../README.md) / [Home](../../../README.md)
