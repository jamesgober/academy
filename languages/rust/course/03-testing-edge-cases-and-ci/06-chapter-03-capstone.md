<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 03](./README.md)

---

# Chapter 03 Capstone: Tested Study Log Library

> Build a small Rust library that is not impressive because it is huge. It is
> impressive because it is clear, tested, documented, and guarded by CI.

This capstone turns the testing chapter into a real artifact:

- A library crate
- Public functions with `Result`-based errors
- Unit tests
- Integration tests
- Doc tests
- Edge-case tests
- A GitHub Actions workflow

---

## What You Are Building

You will build `study-log`, a tiny library for parsing and summarizing study
entries.

Input format:

```text
topic|minutes|notes
```

Example:

```text
Rust|45|ownership and borrowing
Git|20|branching practice
Rust|30|modules and visibility
```

The library should:

- Parse one line into a `StudyEntry`
- Reject invalid lines with useful errors
- Parse many lines into entries
- Total minutes across all entries
- Total minutes for one topic
- Provide public examples in doc tests
- Prove behavior with unit and integration tests

---

## Target Project Shape

```text
study-log/
  Cargo.toml
  src/
    lib.rs
  tests/
    common/
      mod.rs
    public_api.rs
  .github/
    workflows/
      rust.yml
```

Create it:

```bash
cargo new study-log --lib
cd study-log
```

---

## Step 1: Define The Public Types

`src/lib.rs`:

```rust
#[derive(Debug, Clone, PartialEq, Eq)]
pub struct StudyEntry {
    topic: String,
    minutes: u32,
    notes: String,
}

#[derive(Debug, Clone, PartialEq, Eq)]
pub enum ParseStudyEntryError {
    MissingField { field: &'static str },
    EmptyField { field: &'static str },
    InvalidMinutes { value: String },
    TooManyFields,
}
```

The fields on `StudyEntry` are private. That gives the library control over its
rules.

Add accessors:

```rust
impl StudyEntry {
    pub fn new(
        topic: impl Into<String>,
        minutes: u32,
        notes: impl Into<String>,
    ) -> Result<Self, ParseStudyEntryError> {
        let topic = topic.into();
        let notes = notes.into();

        if topic.trim().is_empty() {
            return Err(ParseStudyEntryError::EmptyField { field: "topic" });
        }

        if notes.trim().is_empty() {
            return Err(ParseStudyEntryError::EmptyField { field: "notes" });
        }

        Ok(Self {
            topic,
            minutes,
            notes,
        })
    }

    pub fn topic(&self) -> &str {
        &self.topic
    }

    pub fn minutes(&self) -> u32 {
        self.minutes
    }

    pub fn notes(&self) -> &str {
        &self.notes
    }
}
```

Why use `impl Into<String>`?

It lets callers pass either `String` or `&str`:

```rust
StudyEntry::new("Rust", 45, "ownership")?;

let topic = String::from("Git");
StudyEntry::new(topic, 20, "branching")?;
```

---

## Step 2: Display Errors Clearly

Add:

```rust
impl std::fmt::Display for ParseStudyEntryError {
    fn fmt(&self, formatter: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        match self {
            Self::MissingField { field } => write!(formatter, "missing field: {field}"),
            Self::EmptyField { field } => write!(formatter, "empty field: {field}"),
            Self::InvalidMinutes { value } => {
                write!(formatter, "invalid minutes value: {value}")
            }
            Self::TooManyFields => write!(formatter, "too many fields"),
        }
    }
}

impl std::error::Error for ParseStudyEntryError {}
```

Now the error works with common Rust error-handling tools.

Beginner translation:

- `Debug` is for developers.
- `Display` is for humans.
- `Error` marks the type as a standard Rust error.

---

## Step 3: Parse One Line

Add a public function with a doc test:

```rust
/// Parses one study log line.
///
/// Lines use this format:
///
/// ```text
/// topic|minutes|notes
/// ```
///
/// ```
/// use study_log::{parse_entry, StudyEntry};
///
/// let entry = parse_entry("Rust|45|ownership")?;
///
/// assert_eq!(entry.topic(), "Rust");
/// assert_eq!(entry.minutes(), 45);
/// assert_eq!(entry.notes(), "ownership");
///
/// # Ok::<(), Box<dyn std::error::Error>>(())
/// ```
pub fn parse_entry(line: &str) -> Result<StudyEntry, ParseStudyEntryError> {
    let mut parts = line.split('|');

    let topic = parts
        .next()
        .ok_or(ParseStudyEntryError::MissingField { field: "topic" })?
        .trim();

    let minutes_text = parts
        .next()
        .ok_or(ParseStudyEntryError::MissingField { field: "minutes" })?
        .trim();

    let notes = parts
        .next()
        .ok_or(ParseStudyEntryError::MissingField { field: "notes" })?
        .trim();

    if parts.next().is_some() {
        return Err(ParseStudyEntryError::TooManyFields);
    }

    if minutes_text.is_empty() {
        return Err(ParseStudyEntryError::EmptyField { field: "minutes" });
    }

    let minutes =
        minutes_text
            .parse::<u32>()
            .map_err(|_| ParseStudyEntryError::InvalidMinutes {
                value: minutes_text.to_string(),
            })?;

    StudyEntry::new(topic, minutes, notes)
}
```

Important detail:

```rust
let mut parts = line.split('|');
```

The iterator is mutable because every call to `.next()` advances it.

---

## Step 4: Parse Many Lines

Add:

```rust
/// Parses many study log lines.
///
/// Empty lines are ignored.
pub fn parse_entries(input: &str) -> Result<Vec<StudyEntry>, ParseStudyEntryError> {
    input
        .lines()
        .filter(|line| !line.trim().is_empty())
        .map(parse_entry)
        .collect()
}
```

This line is doing a lot:

```rust
.collect()
```

Because the function returns `Result<Vec<StudyEntry>, ParseStudyEntryError>`,
Rust knows how to collect an iterator of `Result<StudyEntry, ParseStudyEntryError>`.

Mental model:

```text
All lines Ok(entry)       -> Ok(Vec<StudyEntry>)
Any line Err(error)       -> Err(error) immediately
```

---

## Step 5: Summarize Entries

Add:

```rust
pub fn total_minutes(entries: &[StudyEntry]) -> u32 {
    entries.iter().map(StudyEntry::minutes).sum()
}

pub fn total_minutes_for_topic(entries: &[StudyEntry], topic: &str) -> u32 {
    entries
        .iter()
        .filter(|entry| entry.topic().eq_ignore_ascii_case(topic))
        .map(StudyEntry::minutes)
        .sum()
}
```

Why `&[StudyEntry]` instead of `&Vec<StudyEntry>`?

`&[StudyEntry]` accepts:

- A vector slice
- An array slice
- Any borrowed contiguous list of entries

It is more flexible.

---

## Step 6: Add Unit Tests

Put these at the bottom of `src/lib.rs`:

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn parses_valid_entry() {
        let entry = parse_entry("Rust|45|ownership").unwrap();

        assert_eq!(entry.topic(), "Rust");
        assert_eq!(entry.minutes(), 45);
        assert_eq!(entry.notes(), "ownership");
    }

    #[test]
    fn trims_fields() {
        let entry = parse_entry("  Rust  | 45 | ownership  ").unwrap();

        assert_eq!(entry.topic(), "Rust");
        assert_eq!(entry.minutes(), 45);
        assert_eq!(entry.notes(), "ownership");
    }

    #[test]
    fn rejects_empty_topic() {
        let error = parse_entry(" |45|ownership").unwrap_err();

        assert_eq!(
            error,
            ParseStudyEntryError::EmptyField { field: "topic" }
        );
    }

    #[test]
    fn rejects_missing_minutes() {
        let error = parse_entry("Rust").unwrap_err();

        assert_eq!(
            error,
            ParseStudyEntryError::MissingField { field: "minutes" }
        );
    }

    #[test]
    fn rejects_empty_minutes() {
        let error = parse_entry("Rust||ownership").unwrap_err();

        assert_eq!(
            error,
            ParseStudyEntryError::EmptyField { field: "minutes" }
        );
    }

    #[test]
    fn rejects_invalid_minutes() {
        let error = parse_entry("Rust|forty five|ownership").unwrap_err();

        assert_eq!(
            error,
            ParseStudyEntryError::InvalidMinutes {
                value: "forty five".to_string()
            }
        );
    }

    #[test]
    fn rejects_too_many_fields() {
        let error = parse_entry("Rust|45|ownership|extra").unwrap_err();

        assert_eq!(error, ParseStudyEntryError::TooManyFields);
    }

    #[test]
    fn totals_minutes() {
        let entries = parse_entries(
            "\
Rust|45|ownership
Git|20|branching
Rust|30|modules",
        )
        .unwrap();

        assert_eq!(total_minutes(&entries), 95);
    }

    #[test]
    fn totals_minutes_for_topic_case_insensitively() {
        let entries = parse_entries(
            "\
Rust|45|ownership
Git|20|branching
rust|30|modules",
        )
        .unwrap();

        assert_eq!(total_minutes_for_topic(&entries, "RUST"), 75);
    }
}
```

Notice the tests are not random. Each test names one behavior.

---

## Step 7: Add Integration Tests

`tests/common/mod.rs`:

```rust
use study_log::{parse_entries, StudyEntry};

pub fn sample_entries() -> Vec<StudyEntry> {
    parse_entries(
        "\
Rust|45|ownership
Git|20|branching
Rust|30|modules",
    )
    .unwrap()
}
```

`tests/public_api.rs`:

```rust
mod common;

use common::sample_entries;
use study_log::{parse_entries, total_minutes, total_minutes_for_topic};

#[test]
fn public_api_parses_multiple_entries() {
    let entries = parse_entries(
        "\
Rust|45|ownership
Git|20|branching",
    )
    .unwrap();

    assert_eq!(entries.len(), 2);
    assert_eq!(entries[0].topic(), "Rust");
    assert_eq!(entries[1].topic(), "Git");
}

#[test]
fn public_api_totals_all_minutes() {
    let entries = sample_entries();

    assert_eq!(total_minutes(&entries), 95);
}

#[test]
fn public_api_totals_matching_topic() {
    let entries = sample_entries();

    assert_eq!(total_minutes_for_topic(&entries, "Rust"), 75);
}
```

These tests call the crate like a user would.

---

## Step 8: Add CI

`.github/workflows/rust.yml`:

```yaml
name: Rust CI

on:
  pull_request:
  push:
    branches: [main]

jobs:
  checks:
    name: Format, lint, and test
    runs-on: ubuntu-latest

    steps:
      - name: Check out repository
        uses: actions/checkout@v4

      - name: Install stable Rust
        uses: dtolnay/rust-toolchain@stable
        with:
          components: rustfmt, clippy

      - name: Cache Rust build output
        uses: Swatinem/rust-cache@v2

      - name: Check formatting
        run: cargo fmt --all -- --check

      - name: Run Clippy
        run: cargo clippy --all-targets --all-features -- -D warnings

      - name: Run tests
        run: cargo test --all-features

      - name: Run doc tests
        run: cargo test --doc
```

---

## Step 9: Run The Quality Gate

Run:

```bash
cargo fmt --all
cargo clippy --all-targets --all-features -- -D warnings
cargo test --all-features
cargo test --doc
```

Expected:

```text
Formatting completes.
Clippy reports no warnings.
All unit tests pass.
All integration tests pass.
All doc tests pass.
```

If anything fails, use the debugging flow from the previous lesson.

---

## Required Edge Cases

Your final project must test at least these:

| Case | Example | Expected result |
|---|---|---|
| Valid line | `Rust|45|ownership` | `Ok(StudyEntry)` |
| Trimmed fields | ` Rust | 45 | notes ` | Trimmed values |
| Missing minutes | `Rust` | `MissingField { field: "minutes" }` |
| Empty topic | ` |45|notes` | `EmptyField { field: "topic" }` |
| Empty minutes | `Rust||notes` | `EmptyField { field: "minutes" }` |
| Invalid minutes | `Rust|abc|notes` | `InvalidMinutes` |
| Extra field | `Rust|45|notes|x` | `TooManyFields` |
| Empty lines in input | Blank lines | Ignored |
| Topic case | `rust` vs `RUST` | Totals match case-insensitively |

---

## Stretch Goals

After the required version works:

- Add line numbers to parse errors
- Add `from_file(path)` for reading entries from a file
- Add `to_markdown_summary(entries)` for reports
- Add a CLI wrapper in `src/main.rs`
- Add property tests for parse/format round trips
- Add snapshot tests for the markdown summary

Do these after the core project is tested. A messy stretch goal is worse than a
clean small library.

---

## Reviewer Checklist

Use this checklist before calling the capstone done:

- The crate builds from a clean clone.
- Public functions have clear names.
- Invalid input returns `Result::Err`, not a panic.
- Error messages are useful to humans.
- Unit tests cover private module behavior.
- Integration tests cover public API behavior.
- Doc tests compile.
- Edge-case tests are named clearly.
- Tests are deterministic.
- CI runs formatting, Clippy, unit/integration tests, and doc tests.
- The README or project notes show the commands needed to verify the project.

---

## What You Should Understand Now

By completing this capstone, you have practiced:

- Defining public types
- Keeping struct fields private
- Returning `Result`
- Writing custom errors
- Parsing strings
- Collecting `Result` values
- Borrowing slices
- Writing unit tests
- Writing integration tests
- Writing doc tests
- Running CI checks
- Debugging failures

That is real Rust. Not toy Rust. Small, yes. But real.

---

## Next

Continue to [Chapter 04: Concurrency And Async](../04-concurrency-and-async/README.md).

---

[**Next ->** Chapter 04: Concurrency And Async](../04-concurrency-and-async/README.md)  
[**<- Previous** Debugging Test Failures and Flaky Behavior](./05-debugging-test-failures-and-flaky-behavior.md)
