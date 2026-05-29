<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 03](./README.md)

---

# Designing Edge-Case Tests

> Good tests do not only prove the happy path; they guard the weird corners where bugs breed.

**You will learn:**
- how to identify edge-case categories
- how to test boundaries
- how to test invariants
- how to table-test multiple cases
- how to test parser and validation logic like a professional

**Before this page, you should know:** [Rust Testing Foundations](./01-rust-testing-foundations.md)

---

## Edge-Case Categories

| Category | Examples |
|---|---|
| empty input | `""`, empty list, missing file |
| whitespace | `"   "`, `"\n\t"` |
| boundary values | `0`, `1`, max allowed, one above max |
| invalid format | `"abc"` where number expected |
| duplicates | same topic repeated |
| ordering | sorted output, stable grouping |
| large values | huge minutes, long strings |
| Unicode | non-ASCII text |
| failure paths | parse error, file error, permission error |

You do not need infinite tests. You need the right tests around decisions.

## Boundary Tests

```rust
const MAX_DAILY_MINUTES: u32 = 24 * 60;

fn is_valid_minutes(minutes: u32) -> bool {
    minutes > 0 && minutes <= MAX_DAILY_MINUTES
}

#[test]
fn minute_boundaries() {
    assert!(!is_valid_minutes(0));
    assert!(is_valid_minutes(1));
    assert!(is_valid_minutes(MAX_DAILY_MINUTES));
    assert!(!is_valid_minutes(MAX_DAILY_MINUTES + 1));
}
```

Boundaries matter because bugs often happen one step below or above a rule.

## Table-Style Tests

Rust does not need a special framework for table tests.

```rust
#[test]
fn topic_normalization_cases() {
    let cases = [
        ("Rust", Some("Rust")),
        ("  Rust  ", Some("Rust")),
        ("", None),
        ("   ", None),
        ("\t\n", None),
    ];

    for (input, expected) in cases {
        assert_eq!(
            normalize_topic(input).as_deref(),
            expected,
            "input should normalize correctly: {input:?}"
        );
    }
}
```

`as_deref()` turns `Option<String>` into `Option<&str>` for easier comparison.

## Invariants

An invariant is a rule that should always be true.

For `StudyEntry`:

```text
topic is never empty
minutes is greater than zero
minutes is not more than one day
```

Test the constructor because the constructor protects those invariants.

```rust
#[test]
fn constructor_protects_invariants() {
    assert!(StudyEntry::new("Rust", 1).is_ok());
    assert!(StudyEntry::new("", 1).is_err());
    assert!(StudyEntry::new("Rust", 0).is_err());
    assert!(StudyEntry::new("Rust", 24 * 60 + 1).is_err());
}
```

## Parser Tests

Parser tests should cover good lines and bad lines.

```rust
#[test]
fn parses_valid_line() {
    let entry = parse_entry_line("Rust,45").unwrap();

    assert_eq!(entry.topic(), "Rust");
    assert_eq!(entry.minutes(), 45);
}

#[test]
fn rejects_missing_minutes() {
    assert_eq!(
        parse_entry_line("Rust").unwrap_err(),
        ParseEntryError::MissingMinutes
    );
}

#[test]
fn rejects_invalid_minutes() {
    assert_eq!(
        parse_entry_line("Rust,abc").unwrap_err(),
        ParseEntryError::InvalidMinutes
    );
}
```

Do not test only valid examples. Parsers live at messy input boundaries.

## Regression Tests

A regression test is a test for a bug that already happened.

If a user reports that `" Rust , 45 "` failed, add a test:

```rust
#[test]
fn trims_topic_and_minutes_from_line() {
    let entry = parse_entry_line(" Rust , 45 ").unwrap();

    assert_eq!(entry.topic(), "Rust");
    assert_eq!(entry.minutes(), 45);
}
```

Regression tests keep old bugs from quietly returning.

## Reference Links

- [Testing and CI Cheat Sheet](../../reference/testing-and-ci-cheat-sheet.md)
- [Practical Rust Patterns](../../reference/practical-rust-patterns.md)

---

## Recap

- Edge cases live around boundaries, bad input, empty input, and invariants.
- Table-style tests make repeated cases readable.
- Parser tests must include invalid input.
- Regression tests preserve fixes.

## Try It Yourself

Write table-style tests for `parse_entry_line`. Include valid input, trimmed
input, missing comma, empty topic, zero minutes, non-numeric minutes, and a topic
containing Unicode text.

---

[**Next ->** Advanced Testing Options](./03-advanced-testing-options.md)  
[**<- Previous** Rust Testing Foundations](./01-rust-testing-foundations.md)
