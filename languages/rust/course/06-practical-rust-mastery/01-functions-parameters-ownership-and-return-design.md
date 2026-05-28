<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 06](./README.md)

---

# Functions, Parameters, Ownership, and Return Design

> Rust function signatures are small contracts: they tell you who owns data, who may mutate it, what can fail, and what comes back.

**You will learn:**
- how to read Rust function signatures without panic
- when to use owned values, shared references, mutable references, and slices
- how return types communicate success, absence, failure, and mutation
- how to design beginner-friendly APIs that avoid needless cloning
- how to build a useful validation module instead of a four-line toy

**Before this page, you should know:** ownership, borrowing, `Option`, and `Result`.

---

## Read a Signature Like a Sentence

```rust
fn normalize_title(input: &str) -> Option<String>
```

Read it slowly:

- `fn` starts a function.
- `normalize_title` is the function name.
- `input: &str` means the function borrows text and does not own it.
- `-> Option<String>` means it returns either `Some(String)` or `None`.

Plain English: "Give me borrowed text. I may return a cleaned owned string, or I
may return nothing if the input is not usable."

That one line already tells you a lot. Rust wants function boundaries to be
honest.

## Parameter Choices

| Parameter | Meaning | Use when |
|---|---|---|
| `value: String` | function takes ownership | it must store, consume, or transform ownership |
| `value: &str` | function borrows text | it only needs to read text |
| `value: &T` | shared borrow | read without taking ownership |
| `value: &mut T` | mutable borrow | modify caller-owned value |
| `values: &[T]` | borrowed slice | read a list without caring whether caller has array or vector |
| `value: impl Into<String>` | accepts convertible text | ergonomic constructors |

Use the least powerful parameter that does the job. If a function only reads a
title, do not demand ownership of a `String`.

## Bad Beginner Signature

```rust
fn is_valid_title(title: String) -> bool {
    !title.trim().is_empty()
}
```

This works, but it forces the caller to give up ownership.

```rust
let title = String::from("Rust notes");
let valid = is_valid_title(title);
// println!("{title}"); // cannot use title now
```

Better:

```rust
fn is_valid_title(title: &str) -> bool {
    !title.trim().is_empty()
}
```

Now callers can pass string literals, `String`, or borrowed slices:

```rust
let owned = String::from("Rust notes");

assert!(is_valid_title("literal"));
assert!(is_valid_title(&owned));
assert!(is_valid_title(owned.as_str()));
```

## Return Design

| Return type | Meaning | Example |
|---|---|---|
| `()` | did work, no useful value | print/log/update |
| `T` | always returns a value | `fn double(n: u32) -> u32` |
| `Option<T>` | value may be absent | parse optional search field |
| `Result<T, E>` | operation may fail | parse file, validate input |
| `Vec<T>` | returns a new list | filter/transform data |
| `&T` | returns borrowed existing data | find item inside input |

If failure is expected and useful to the caller, return `Result`. If absence is
normal and not an error, return `Option`.

## Build a Real Validation Module

Imagine a study log app. Users enter topics and minutes studied. We need clean
data before writing anything to a file.

```rust
#[derive(Debug, Clone, PartialEq, Eq)]
pub struct StudyEntry {
    topic: String,
    minutes: u32,
}

#[derive(Debug, Clone, PartialEq, Eq)]
pub enum EntryError {
    EmptyTopic,
    InvalidMinutes,
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

pub fn normalize_topic(topic: &str) -> Option<String> {
    let clean = topic.trim();

    if clean.is_empty() {
        None
    } else {
        Some(clean.to_owned())
    }
}
```

Why this is good Rust:

- `StudyEntry::new` owns the final clean data.
- `normalize_topic` borrows input because it only needs to inspect it.
- `Option<String>` fits "empty topic produces no clean topic."
- `Result<Self, EntryError>` fits "entry creation can fail for named reasons."
- Fields are private, so invalid entries cannot be built directly.
- Getters return borrowed or copied values without exposing mutation.

## `impl Into<String>` Without Magic

This signature:

```rust
pub fn new(topic: impl Into<String>, minutes: u32) -> Result<Self, EntryError>
```

lets callers pass either:

```rust
StudyEntry::new("Rust ownership", 45)?;
StudyEntry::new(String::from("Borrowing"), 30)?;
```

Inside the function, `topic.into()` converts the input into a `String`.

Do not use `impl Into<String>` everywhere. Use it mostly for constructors and
APIs that store owned text.

## Mutating Through a Function

Sometimes mutation is exactly the right tool.

```rust
pub fn add_minutes(entry: &mut StudyEntry, extra: u32) -> Result<(), EntryError> {
    let next = entry.minutes + extra;

    if next > 24 * 60 {
        return Err(EntryError::InvalidMinutes);
    }

    entry.minutes = next;
    Ok(())
}
```

`&mut StudyEntry` tells the caller, "this function may change your entry."

```rust
let mut entry = StudyEntry::new("Rust", 30)?;
add_minutes(&mut entry, 15)?;
```

The `mut` at the call site makes mutation visible.

## Borrowed Return Values

This function returns a reference into the input slice:

```rust
pub fn longest_topic<'a>(entries: &'a [StudyEntry]) -> Option<&'a StudyEntry> {
    entries.iter().max_by_key(|entry| entry.topic().len())
}
```

Plain English: "If there is an entry, return a borrowed entry that lives no
longer than the input slice."

Rust often infers lifetimes, so this can usually be written:

```rust
pub fn longest_topic(entries: &[StudyEntry]) -> Option<&StudyEntry> {
    entries.iter().max_by_key(|entry| entry.topic().len())
}
```

The explicit version teaches what is happening.

## Mini Project: Study Entry Validator

Create `src/lib.rs` with the code above. Add tests:

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn trims_topic() {
        let entry = StudyEntry::new("  Rust  ", 25).unwrap();

        assert_eq!(entry.topic(), "Rust");
        assert_eq!(entry.minutes(), 25);
    }

    #[test]
    fn rejects_empty_topic() {
        let error = StudyEntry::new("   ", 25).unwrap_err();

        assert_eq!(error, EntryError::EmptyTopic);
    }

    #[test]
    fn rejects_zero_minutes() {
        let error = StudyEntry::new("Rust", 0).unwrap_err();

        assert_eq!(error, EntryError::InvalidMinutes);
    }
}
```

Run:

```bash
cargo test
```

## Reference Links

- [Functions, Methods, Generics, and Traits](../../reference/functions-methods-generics-and-traits.md)
- [Ownership and Borrowing Cheat Sheet](../../reference/ownership-and-borrowing-cheat-sheet.md)
- [Errors, Warnings, and Debugging](../../reference/errors-warnings-and-debugging.md)

---

## Recap

- Function signatures describe ownership, mutation, absence, and failure.
- Prefer `&str` when reading text; use `String` when storing owned text.
- Prefer slices like `&[T]` when reading lists.
- Use `Option` for absence and `Result` for meaningful failure.
- Good Rust APIs make invalid states difficult to create.

## Try It Yourself

Add `StudyEntry::rename(&mut self, topic: impl Into<String>) -> Result<(), EntryError>`.
It should trim the new topic, reject empty topics, and leave the old topic
unchanged if validation fails.

---

[**Next ->** Types, Strings, Collections, and Data Work](./02-types-strings-collections-and-data-work.md)  
[**<- Previous** Practical Rust Mastery](./README.md)
