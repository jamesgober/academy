<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 03](./README.md)

---

# Advanced Testing Options

> Rust's built-in test runner is enough for a lot of real software. Advanced
> testing is not about collecting fancy tools. It is about choosing the right
> kind of proof for the risk in front of you.

**You will learn:**
- How doc tests turn examples into checked documentation
- When to use ignored tests, panic tests, helper modules, and integration tests
- What property testing, fuzzing, and snapshot testing are for
- How to decide whether an external testing crate is worth adding

**Before this page, you should know:** [Designing Edge-Case Tests](./02-designing-edge-case-tests.md)

---

## The Testing Ladder

When you are new, it is tempting to ask, "What is the best testing tool?"

The better question is:

> What kind of mistake am I trying to catch?

Use this ladder:

```text
Fastest / simplest

1. Unit tests
   Catch wrong logic inside one function or module.

2. Integration tests
   Catch wrong public API behavior from the outside.

3. Doc tests
   Catch examples that no longer compile or no longer tell the truth.

4. Table tests
   Catch many examples of the same rule.

5. Property tests
   Catch rule-breaking cases you did not think to write by hand.

6. Fuzz tests
   Attack parsers and input-heavy code with strange generated input.

7. Snapshot tests
   Detect changes in large text, JSON, HTML, CLI output, or serialized data.

Slowest / most specialized
```

You do not need every rung on every project. A tiny calculator function may only
need unit tests. A parser that accepts user files deserves more.

---

## Doc Tests

Doc tests are examples inside documentation comments that Rust compiles and runs
when you run `cargo test`.

That means your examples do not quietly rot.

### A Function With A Checked Example

```rust
/// Converts a title into a simple URL slug.
///
/// ```
/// use study_tools::slugify;
///
/// assert_eq!(slugify("Rust Basics"), "rust-basics");
/// assert_eq!(slugify("  Borrowing & Ownership!  "), "borrowing-ownership");
/// ```
pub fn slugify(input: &str) -> String {
    input
        .trim()
        .to_lowercase()
        .chars()
        .filter_map(|ch| {
            if ch.is_ascii_alphanumeric() {
                Some(ch)
            } else if ch.is_whitespace() || ch == '-' {
                Some('-')
            } else {
                None
            }
        })
        .collect::<String>()
        .split('-')
        .filter(|part| !part.is_empty())
        .collect::<Vec<_>>()
        .join("-")
}
```

If this code lives in a library crate named `study_tools`, the doc example can
import the public function exactly like a user would.

### Why Doc Tests Matter

Doc tests are excellent for:

- Public functions
- Public structs and enums
- Library crates
- Small "how do I use this?" examples
- Teaching comments beside tricky APIs

They are weaker for:

- Private helpers
- Long multi-file workflows
- Code that needs a database, network, or local machine setup

Use doc tests when the reader deserves a tiny working example.

### Running Only Doc Tests

```bash
cargo test --doc
```

Use this when you changed documentation examples and want quick feedback.

---

## Testing Panic Behavior

Most beginner Rust code should return `Result`, not panic. A panic means the
program hit a bug or an unrecoverable assumption.

Still, some APIs intentionally panic. For example, indexing a slice out of range
panics because there is no element to return.

Rust lets you test this with `#[should_panic]`.

```rust
pub fn percentage(score: u32, max: u32) -> u32 {
    assert!(max > 0, "max score must be greater than zero");
    score * 100 / max
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn calculates_percentage() {
        assert_eq!(percentage(8, 10), 80);
    }

    #[test]
    #[should_panic(expected = "max score must be greater than zero")]
    fn panics_when_max_is_zero() {
        percentage(8, 0);
    }
}
```

The `expected` text is important. Without it, the test passes for any panic,
even the wrong one.

### When To Avoid `should_panic`

Prefer `Result` when the caller can do something useful about the problem:

```rust
pub fn percentage(score: u32, max: u32) -> Result<u32, String> {
    if max == 0 {
        return Err("max score must be greater than zero".to_string());
    }

    Ok(score * 100 / max)
}
```

Now the caller can show a message, ask for another value, or log the issue.

Rule of thumb:

```text
Bad user input?       Return Result.
Missing file?         Return Result.
Network failed?       Return Result.
Impossible bug state? Panic or assert.
```

---

## Ignored Tests

Some tests are useful but too slow or too environment-specific to run every time.

Mark them with `#[ignore]`.

```rust
#[test]
#[ignore = "downloads a large fixture; run manually before release"]
fn parses_full_real_world_export() {
    let data = include_str!("fixtures/large-export.txt");
    let records = parse_export(data).expect("fixture should parse");

    assert!(records.len() > 10_000);
}
```

Normal test run:

```bash
cargo test
```

Run ignored tests too:

```bash
cargo test -- --ignored
```

Run both normal and ignored tests:

```bash
cargo test -- --include-ignored
```

Use ignored tests for:

- Long-running checks
- Tests that need a special local fixture
- Manual release checks
- Expensive compatibility checks

Do not hide broken tests with `#[ignore]`. If a test is flaky or failing, fix the
cause or delete the test. Ignored tests should be intentional.

---

## Shared Test Helpers

Integration tests live in `tests/`. Each file in `tests/` is compiled as a
separate crate. That means shared helpers need a small module.

Example layout:

```text
study-tools/
  Cargo.toml
  src/
    lib.rs
  tests/
    common/
      mod.rs
    parse_study_log.rs
    summarize_study_log.rs
```

`tests/common/mod.rs`:

```rust
use study_tools::StudyEntry;

pub fn sample_entries() -> Vec<StudyEntry> {
    vec![
        StudyEntry::new("Rust", 45, "ownership").unwrap(),
        StudyEntry::new("Rust", 30, "borrowing").unwrap(),
        StudyEntry::new("Git", 20, "commits").unwrap(),
    ]
}
```

`tests/summarize_study_log.rs`:

```rust
mod common;

use common::sample_entries;
use study_tools::total_minutes_for_topic;

#[test]
fn totals_minutes_for_matching_topic() {
    let entries = sample_entries();

    assert_eq!(total_minutes_for_topic(&entries, "Rust"), 75);
}
```

The `common` folder pattern keeps Cargo from treating `common.rs` as its own
integration test file.

---

## Property Testing

Property testing checks rules instead of only checking hand-picked examples.

A normal test says:

> `reverse("abc")` should be `"cba"`.

A property test says:

> For any string, reversing twice should produce the original string.

That rule catches more cases:

```text
""             -> ""
"a"            -> "a"
"hello"        -> "hello"
"racecar"      -> "racecar"
"rust 🦀"      -> "rust 🦀"
```

In Rust, property testing usually uses an external crate such as `proptest` or
`quickcheck`. You do not need those crates yet to understand the idea.

Manual version:

```rust
fn reverse(input: &str) -> String {
    input.chars().rev().collect()
}

#[test]
fn reversing_twice_returns_original_text() {
    let examples = ["", "a", "hello", "racecar", "Rust 2026"];

    for example in examples {
        let reversed = reverse(example);
        let reversed_twice = reverse(&reversed);

        assert_eq!(reversed_twice, example);
    }
}
```

A property-testing crate can generate many examples for you, including weird
ones you did not think of.

### Good Property Test Targets

Property testing fits code with durable rules:

- Sorting: output is ordered and has the same items as input
- Encoding/decoding: decoding encoded data returns the original
- Parsing/formatting: parsing formatted data returns the same value
- Math: operations obey known relationships
- Validation: accepted values always satisfy the invariant

Property testing is less helpful when:

- The correct answer is a specific design choice
- The behavior is mostly visual
- The function depends heavily on external services
- The rule is too vague to express

---

## Fuzz Testing

Fuzz testing throws huge amounts of generated input at your code to find crashes,
panics, hangs, or security problems.

It is especially useful for:

- Parsers
- File readers
- Protocol handlers
- Compilers and interpreters
- Anything that accepts untrusted input

For example, imagine a parser:

```rust
pub fn parse_entry(line: &str) -> Result<StudyEntry, ParseError> {
    // Parses: topic|minutes|notes
    todo!("parser from earlier lessons")
}
```

A fuzz test would feed it thousands or millions of strange strings:

```text
""
"||||"
"\0\0\0"
"Rust|999999999999999999999999|x"
"Rust|-5|x"
"a".repeat(10_000_000)
random bytes that are not valid UTF-8
```

The goal is not "every input should succeed." The goal is:

```text
Valid input      -> returns Ok(value)
Invalid input    -> returns Err(reason)
Any input        -> does not panic, corrupt memory, or hang forever
```

You usually add fuzzing after a parser has normal tests and edge-case tests.

---

## Snapshot Testing

Snapshot testing saves expected output and compares future output against it.

It can be useful for:

- CLI help text
- Generated markdown
- JSON output
- HTML fragments
- Error reports
- Long formatted summaries

Example output:

```text
Study Summary
=============
Rust: 75 minutes
Git: 20 minutes
Total: 95 minutes
```

Instead of writing many individual assertions, a snapshot test can compare the
whole output.

Tradeoffs:

- Good: catches accidental text changes
- Good: makes big output easier to review
- Risky: reviewers may approve snapshot updates without reading them
- Risky: brittle if output changes often for harmless reasons

Beginner rule:

> Use normal assertions first. Reach for snapshots when the output is large,
> stable, and meaningful as a whole.

---

## Dependency Decision Model

Before adding a testing crate, ask:

```text
1. What exact problem does this crate solve?
2. Can standard Rust testing solve it clearly enough?
3. Will beginners understand the test after the crate is introduced?
4. Is the crate maintained?
5. Does it make failures easier to debug?
6. Can CI run it quickly and reliably?
```

Good dependency:

```text
"We are writing a parser. Hand-written edge tests are not enough.
Property tests will generate many valid and invalid records and catch cases
we did not think of."
```

Weak dependency:

```text
"This crate has a nicer assertion style."
```

Nice is not bad, but it is not always worth more moving parts.

---

## Real-World Testing Mix

For a small Rust library:

```text
src/lib.rs
  Unit tests for private helper behavior
  Doc tests for public examples

tests/public_api.rs
  Integration tests for how users call the library

.github/workflows/rust.yml
  fmt, clippy, test, doc test
```

For a CLI:

```text
src/
  Unit tests for parsing and formatting

tests/
  Integration tests for library behavior
  Optional command tests for CLI output

fixtures/
  Small input files with expected results

.github/workflows/rust.yml
  fmt, clippy, test
```

For a parser that reads user files:

```text
Standard tests
  Happy path
  Missing fields
  Bad numbers
  Empty input
  Huge input

Advanced tests
  Property tests for round-trip behavior
  Fuzz tests for crash resistance
```

---

## Mini Project: Document Your Public API

Take this function:

```rust
pub fn total_minutes(minutes: &[u32]) -> u32 {
    minutes.iter().sum()
}
```

Add a doc test:

```rust
/// Adds study session minutes together.
///
/// ```
/// use study_tools::total_minutes;
///
/// let sessions = [20, 35, 10];
/// assert_eq!(total_minutes(&sessions), 65);
/// ```
pub fn total_minutes(minutes: &[u32]) -> u32 {
    minutes.iter().sum()
}
```

Then run:

```bash
cargo test --doc
```

If the example imports the wrong crate name, Rust will tell you. That is the
point. Documentation should fail loudly when it stops being true.

---

## Chapter Checkpoint

You should now be able to answer:

- What is the difference between a unit test, integration test, and doc test?
- When should a panic test use `expected = "..."`?
- Why are ignored tests not a place to hide broken behavior?
- What kind of code benefits from property testing?
- What kind of code benefits from fuzzing?
- What tradeoff makes snapshot tests risky?

---

## Recap

- Start with built-in tests because they are fast, stable, and easy to explain.
- Use doc tests to keep public examples honest.
- Use ignored tests only for intentional slow or manual checks.
- Use property tests and fuzzing when hand-picked examples are not enough.
- Use snapshot tests carefully for large, stable output.
- Add dependencies because they reduce real risk, not because they look fancy.

## Try It Yourself

Pick one module from your Rust project and write a testing plan:

```text
Module:
Main risk:
Unit tests needed:
Integration tests needed:
Doc tests needed:
Advanced tests worth considering:
Advanced tests not worth adding yet:
Reason:
```

Then add at least one doc test to a public function.

---

[**Next ->** Rust CI Workflows with GitHub Actions](./04-rust-ci-workflows-with-github-actions.md)  
[**<- Previous** Designing Edge-Case Tests](./02-designing-edge-case-tests.md)
