<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 03](./README.md)

---

# Rust Testing Foundations

> Rust makes testing a normal part of the language workflow, not an afterthought.

**You will learn:**
- how `cargo test` discovers and runs tests
- where unit tests and integration tests live
- how assertions work
- how to test success and failure
- how to test private helpers without making them public
- how to run one test, show output, and read failures

**Before this page, you should know:** [Chapter 02 Checkpoint](../02-rust-core-mental-model/07-chapter-02-checkpoint.md)

---

## The Smallest Test

```rust
pub fn add(left: i32, right: i32) -> i32 {
    left + right
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn adds_two_numbers() {
        assert_eq!(add(2, 3), 5);
    }
}
```

Run:

```bash
cargo test
```

What each piece means:

- `#[cfg(test)]` compiles the test module only during tests.
- `mod tests` groups tests near the code they test.
- `use super::*;` imports the parent module's items.
- `#[test]` marks a function as a test.
- `assert_eq!` fails the test if values differ.

## Test Output

Typical passing output:

```text
running 1 test
test tests::adds_two_numbers ... ok

test result: ok. 1 passed; 0 failed
```

Typical failure:

```text
thread 'tests::adds_two_numbers' panicked at src/lib.rs:10:9:
assertion `left == right` failed
  left: 5
 right: 6
```

Read it as:

```text
which test failed
where it failed
what assertion failed
actual and expected values
```

## Unit Tests Versus Integration Tests

Unit tests live next to the code, usually in the same file.

```text
src/
`-- lib.rs  <- unit tests inside #[cfg(test)] mod tests
```

Integration tests live in `tests/`.

```text
tests/
`-- study_entry_tests.rs
```

Integration tests use your crate like an outside user:

```rust
use study_log::StudyEntry;

#[test]
fn creates_valid_entry() {
    let entry = StudyEntry::new("Rust", 45).unwrap();

    assert_eq!(entry.topic(), "Rust");
}
```

Use unit tests for small internal logic. Use integration tests for public API
behavior.

## Test Failures with `Result`

```rust
#[derive(Debug, PartialEq, Eq)]
enum EntryError {
    EmptyTopic,
}

fn require_topic(topic: &str) -> Result<String, EntryError> {
    let topic = topic.trim();

    if topic.is_empty() {
        Err(EntryError::EmptyTopic)
    } else {
        Ok(topic.to_owned())
    }
}

#[test]
fn rejects_empty_topic() {
    let error = require_topic("   ").unwrap_err();

    assert_eq!(error, EntryError::EmptyTopic);
}
```

`unwrap_err()` is useful in tests when the test is specifically proving that an
error happens.

## Assertion Tools

| Macro | Use |
|---|---|
| `assert!(condition)` | condition should be true |
| `assert_eq!(left, right)` | values should be equal |
| `assert_ne!(left, right)` | values should differ |
| `panic!("message")` | fail manually |

Custom message:

```rust
assert!(
    minutes <= 24 * 60,
    "minutes should fit inside one day"
);
```

## Run Specific Tests

```bash
cargo test creates_valid_entry
cargo test entry
cargo test -- --nocapture
```

`-- --nocapture` shows `println!` output from tests. Use it while debugging.

## Testing Private Helpers

Unit tests inside the same module can access private items.

```rust
fn normalize_topic(topic: &str) -> Option<String> {
    let topic = topic.trim();

    if topic.is_empty() {
        None
    } else {
        Some(topic.to_owned())
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn trims_topic() {
        assert_eq!(normalize_topic(" Rust "), Some(String::from("Rust")));
    }
}
```

Do not make helpers public only so tests can reach them. Put unit tests near the
helper.

## Real Test Set

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn accepts_clean_topic() {
        assert_eq!(normalize_topic("Rust"), Some(String::from("Rust")));
    }

    #[test]
    fn trims_topic() {
        assert_eq!(normalize_topic("  Rust  "), Some(String::from("Rust")));
    }

    #[test]
    fn rejects_empty_topic() {
        assert_eq!(normalize_topic("   "), None);
    }
}
```

These tests cover normal, cleanup, and rejection behavior.

## Reference Links

- [Testing and CI Cheat Sheet](../../reference/testing-and-ci-cheat-sheet.md)
- [Errors, Warnings, and Debugging](../../reference/errors-warnings-and-debugging.md)

---

## Recap

- `cargo test` runs Rust tests.
- Unit tests live near the code and can test private helpers.
- Integration tests live in `tests/` and use the public API.
- Test both success and failure.
- Use targeted test commands to debug faster.

## Try It Yourself

Create a `StudyEntry::new` constructor and write tests for:

- valid entry
- trimmed topic
- empty topic rejected
- zero minutes rejected
- huge minutes rejected

---

[**Next ->** Designing Edge-Case Tests](./02-designing-edge-case-tests.md)  
[**<- Previous** Chapter Testing, Edge Cases, and CI](./README.md)
