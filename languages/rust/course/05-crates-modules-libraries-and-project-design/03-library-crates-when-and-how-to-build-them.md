<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 05](./README.md)

---

# Library Crates: When And How To Build Them

> A library crate is where your reusable Rust knowledge lives. The binary should
> ask the library to do work; the library should not need to know how the binary
> prints, exits, or parses command-line text.

**You will learn:**
- When a library crate is worth creating
- How to design a small public API
- How to keep internals private
- How to document public functions with examples
- How to test the library from the outside

**Before this page, you should know:** [Modules, Visibility, And Exports](./02-modules-and-visibility.md)

---

## When To Make A Library

Create a library crate when:

- Multiple binaries need the same logic
- You want integration tests to call public APIs
- The code might be reused by another project
- The code has rules worth separating from input/output
- `main.rs` is becoming a wall of logic

Do not create a library only because every serious project has one. Create a
library when it makes code easier to test, reuse, and explain.

---

## The Good Split

```text
src/
  lib.rs    reusable logic
  main.rs   command-line wiring
```

Library responsibilities:

- Models
- Validation
- Parsing
- Calculations
- Business rules
- Public error types
- Public functions

Binary responsibilities:

- Read command-line args
- Read environment variables
- Read files
- Call library functions
- Print output
- Choose process exit code

Visual model:

```text
main.rs
  parse input
  call library
  print result

lib.rs
  understand the domain
  return values and errors
```

---

## Build A Tiny Library

Create:

```bash
cargo new gradebook --lib
cd gradebook
```

`src/lib.rs`:

```rust
#[derive(Debug, Clone, PartialEq, Eq)]
pub struct Assignment {
    name: String,
    score: u32,
    max_score: u32,
}

#[derive(Debug, Clone, PartialEq, Eq)]
pub enum AssignmentError {
    EmptyName,
    ZeroMaxScore,
    ScoreAboveMax { score: u32, max_score: u32 },
}
```

Private fields force valid construction through your API.

Add constructor and accessors:

```rust
impl Assignment {
    pub fn new(
        name: impl Into<String>,
        score: u32,
        max_score: u32,
    ) -> Result<Self, AssignmentError> {
        let name = name.into();

        if name.trim().is_empty() {
            return Err(AssignmentError::EmptyName);
        }

        if max_score == 0 {
            return Err(AssignmentError::ZeroMaxScore);
        }

        if score > max_score {
            return Err(AssignmentError::ScoreAboveMax { score, max_score });
        }

        Ok(Self {
            name,
            score,
            max_score,
        })
    }

    pub fn name(&self) -> &str {
        &self.name
    }

    pub fn score(&self) -> u32 {
        self.score
    }

    pub fn max_score(&self) -> u32 {
        self.max_score
    }
}
```

This is a Rust library habit:

```text
Keep fields private.
Expose safe constructors.
Expose accessors for what callers need.
```

---

## Add Public Behavior

```rust
/// Calculates an assignment percentage.
///
/// ```
/// use gradebook::{percentage, Assignment};
///
/// let assignment = Assignment::new("Quiz 1", 8, 10)?;
///
/// assert_eq!(percentage(&assignment), 80);
///
/// # Ok::<(), Box<dyn std::error::Error>>(())
/// ```
pub fn percentage(assignment: &Assignment) -> u32 {
    assignment.score() * 100 / assignment.max_score()
}

pub fn average_percentage(assignments: &[Assignment]) -> Option<u32> {
    if assignments.is_empty() {
        return None;
    }

    let total: u32 = assignments.iter().map(percentage).sum();
    Some(total / assignments.len() as u32)
}
```

Why return `Option<u32>` for `average_percentage`?

An empty list has no average. Returning `None` is clearer than pretending the
average is `0`.

---

## Public API Surface

Everything marked `pub` becomes part of what callers can use.

Beginner mistake:

```rust
pub mod internal_parser_helpers;
pub struct Assignment {
    pub name: String,
    pub score: u32,
    pub max_score: u32,
}
```

This exposes too much. Callers can create invalid assignments:

```rust
let broken = Assignment {
    name: String::new(),
    score: 999,
    max_score: 0,
};
```

Better:

```rust
pub struct Assignment {
    name: String,
    score: u32,
    max_score: u32,
}
```

Now callers must use `Assignment::new`.

Public API rule:

> Make invalid states hard to create.

---

## Re-Exports For Clean Imports

If the project grows:

```text
src/
  lib.rs
  assignment.rs
  report.rs
```

`src/assignment.rs`:

```rust
#[derive(Debug, Clone, PartialEq, Eq)]
pub struct Assignment {
    name: String,
    score: u32,
    max_score: u32,
}
```

`src/lib.rs`:

```rust
mod assignment;
mod report;

pub use assignment::{Assignment, AssignmentError};
pub use report::{average_percentage, percentage};
```

Callers get:

```rust
use gradebook::{Assignment, percentage};
```

Instead of:

```rust
use gradebook::assignment::Assignment;
use gradebook::report::percentage;
```

The library keeps its internal file layout private.

---

## Test From Inside And Outside

Unit tests can live near implementation:

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn rejects_score_above_max() {
        let error = Assignment::new("Quiz", 11, 10).unwrap_err();

        assert_eq!(
            error,
            AssignmentError::ScoreAboveMax {
                score: 11,
                max_score: 10
            }
        );
    }
}
```

Integration tests live in `tests/` and use the public API:

```text
tests/
  gradebook_api.rs
```

```rust
use gradebook::{average_percentage, Assignment};

#[test]
fn averages_public_assignments() {
    let assignments = vec![
        Assignment::new("Quiz 1", 8, 10).unwrap(),
        Assignment::new("Quiz 2", 9, 10).unwrap(),
    ];

    assert_eq!(average_percentage(&assignments), Some(85));
}
```

If an integration test cannot reach something, ask:

```text
Should this be public?
Or should the public API expose a better behavior?
```

Do not make internals public just to test them from the outside.

---

## Library Documentation Habits

Public items should answer:

- What does this do?
- What inputs are accepted?
- What errors can happen?
- What does a small example look like?

Example:

```rust
/// Builds a valid assignment.
///
/// Returns an error when the name is empty, the max score is zero, or the score
/// is greater than the max score.
```

Run doc tests:

```bash
cargo test --doc
```

Generate local docs:

```bash
cargo doc --open
```

Reading your own generated docs is humbling in the most useful way. If the docs
feel awkward, the API may be awkward.

---

## Mini Project: Extract A Library

Start with a messy `main.rs`:

```rust
fn main() {
    let scores = vec![8, 9, 10];
    let total: u32 = scores.iter().sum();
    let average = total / scores.len() as u32;

    println!("average: {average}");
}
```

Refactor to:

```text
src/
  lib.rs
  main.rs
```

`src/lib.rs`:

```rust
pub fn average(scores: &[u32]) -> Option<u32> {
    if scores.is_empty() {
        return None;
    }

    let total: u32 = scores.iter().sum();
    Some(total / scores.len() as u32)
}
```

`src/main.rs`:

```rust
use gradebook::average;

fn main() {
    let scores = vec![8, 9, 10];

    match average(&scores) {
        Some(value) => println!("average: {value}"),
        None => println!("no scores"),
    }
}
```

Test:

```rust
#[test]
fn empty_scores_have_no_average() {
    assert_eq!(average(&[]), None);
}
```

---

## Chapter Checkpoint

You should now be able to answer:

- Why move reusable logic out of `main.rs`?
- What belongs in a library crate?
- What belongs in a binary crate?
- Why keep struct fields private?
- What is a re-export?
- Why are doc tests useful for public APIs?
- Why should integration tests use the library like a real caller?

---

## Recap

- Library crates hold reusable, testable logic.
- Binary crates wire inputs and outputs to library behavior.
- Public API should be small, documented, and hard to misuse.
- Re-exports let you keep clean caller imports while reorganizing internals.
- Good libraries are designed around useful behavior, not file layout.

## Try It Yourself

Take one previous Rust mini project and split it into:

- `src/lib.rs` for domain logic
- `src/main.rs` for printing and orchestration
- `tests/public_api.rs` for integration tests

Run `cargo test` and `cargo test --doc`.

---

[**Next ->** Binary Crates: Applications And Entry Points](./04-binary-crates-applications-and-entry-points.md)  
[**<- Previous** Modules, Visibility, And Exports](./02-modules-and-visibility.md)
