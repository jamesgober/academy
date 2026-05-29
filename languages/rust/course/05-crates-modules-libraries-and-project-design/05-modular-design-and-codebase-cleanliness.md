<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 05](./README.md)

---

# Modular Design And Codebase Cleanliness

> Clean Rust code is not code with the fewest files. Clean Rust code is code
> where each file has a clear job and the public API is easy to understand.

**You will learn:**
- How to spot messy boundaries early
- How to decide when to split a module
- How to keep public APIs smaller than internal structure
- How to avoid circular design pressure
- How to organize tests around behavior
- How to grow a Rust project without losing the map

**Before this page, you should know:** [Binary Crates: Applications And Entry Points](./04-binary-crates-applications-and-entry-points.md)

---

## Clean Codebase Habits

Good Rust modules usually have:

- One clear responsibility
- Private internals by default
- Public functions that describe behavior
- Types that protect invariants
- Tests near important rules
- File names that match concepts
- Minimal knowledge of unrelated modules

Weak modules often have:

- Vague names like `utils`, `helpers`, or `misc`
- Many unrelated public functions
- Public fields everywhere
- Repeated validation logic
- Functions that read input, mutate state, and print output all at once
- Tests that depend on implementation details

`utils` is not always evil, but it often means:

```text
I did not know where this belongs.
```

When a utility grows a theme, name the theme.

---

## Boundary Sketch

For a CLI project:

```text
Binary crate
  src/main.rs
    CLI args
    printing
    exit codes

Library crate
  src/lib.rs
    public exports

  src/model.rs
    domain types

  src/parser.rs
    text -> domain types

  src/report.rs
    domain types -> output strings

  src/error.rs
    public error types
```

The binary should not know how parsing is internally split. It should call the
public API.

---

## Public API Smaller Than Internal Design

Internal structure:

```text
src/
  lib.rs
  model.rs
  parser.rs
  parser/
    lexer.rs
    grammar.rs
  report.rs
```

Public API:

```rust
pub use model::StudyEntry;
pub use parser::{parse_entry, parse_entries, ParseError};
pub use report::render_summary;
```

Callers see:

```rust
use study_log::{parse_entries, render_summary};
```

They do not need:

```rust
use study_log::parser::grammar::parse_line;
```

Design rule:

> Your public API should expose what callers need to do, not how your files are
> arranged.

---

## When To Split A Module

Split when:

- The file is hard to scan
- Several sections have different reasons to change
- Tests naturally group around separate behaviors
- Names inside the file need repeated prefixes
- You can describe the new module with a specific noun

Do not split only because a file passed some arbitrary line count.

Example:

```text
parser.rs
  parse_entry
  parse_minutes
  parse_topic
  parse_notes
  ParseError
```

This may be fine.

Later:

```text
parser/
  mod.rs
  error.rs
  line.rs
  document.rs
```

This split makes sense if line parsing and document parsing each become
substantial.

---

## Module Names

Prefer names from the problem domain:

Good:

```text
inventory
invoice
parser
report
session
student
assignment
```

Usually weak:

```text
utils
helpers
common
stuff
manager
processor
```

Some generic names are acceptable in context. For example, `common` in `tests/`
is a known test helper pattern. But in application code, specific names usually
age better.

---

## Avoid Circular Design Pressure

Rust modules cannot depend on each other in a circular way like this:

```text
parser needs report
report needs parser
```

Even when you can technically route imports around it, the design smell remains.

Break the cycle by extracting a shared concept:

```text
model
  StudyEntry

parser
  text -> StudyEntry

report
  StudyEntry -> text summary
```

Visual model:

```text
parser ----\
            -> model -> report
input -----/            output
```

The model becomes the stable middle.

---

## Keep I/O At The Edges

Messy:

```rust
pub fn summarize_file(path: &str) {
    let text = std::fs::read_to_string(path).unwrap();
    let entries = parse_entries(&text).unwrap();
    println!("{}", render_summary(&entries));
}
```

This function:

- Reads files
- Parses text
- Handles errors by panicking
- Prints output

Hard to test. Hard to reuse.

Cleaner:

```rust
pub fn summarize_text(text: &str) -> Result<String, ParseError> {
    let entries = parse_entries(text)?;
    Ok(render_summary(&entries))
}
```

Then the binary handles the file:

```rust
fn run(path: &str) -> Result<(), Box<dyn std::error::Error>> {
    let text = std::fs::read_to_string(path)?;
    let summary = study_log::summarize_text(&text)?;

    println!("{summary}");

    Ok(())
}
```

The library owns rules. The binary owns I/O.

---

## Architecture Smells And Refactors

| Smell | What it usually means | Refactor |
|---|---|---|
| Giant `main.rs` | App wiring and logic are mixed | Move rules into `lib.rs` |
| Huge `utils.rs` | Missing domain names | Split into named modules |
| Public fields everywhere | Invariants are unprotected | Use constructors and accessors |
| Repeated validation | Rules are scattered | Centralize in type constructors |
| Tests require private details | API may be awkward | Test public behavior or move unit tests closer |
| Many modules import each other | Boundaries are blurry | Extract shared model types |
| Function does I/O and logic | Hard to test | Split pure logic from side effects |

---

## Tests Follow Boundaries

Use unit tests for internal rules:

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn rejects_empty_topic() {
        assert!(parse_topic(" ").is_err());
    }
}
```

Use integration tests for public workflows:

```rust
use study_log::{parse_entries, render_summary};

#[test]
fn parses_and_summarizes_entries() {
    let entries = parse_entries("Rust|45|ownership").unwrap();
    let summary = render_summary(&entries);

    assert!(summary.contains("Rust"));
}
```

Do not make a private helper public only because an integration test wants it.
Move the test closer or test the public behavior.

---

## Refactoring Plan For A Messy File

When a file is messy:

1. List the responsibilities in the file.
2. Group related functions and types.
3. Name each group with a domain word.
4. Move one group into a module.
5. Keep public exports stable if callers rely on them.
6. Run tests.
7. Repeat.

Example responsibility list:

```text
main.rs currently does:
  read CLI args
  read file
  parse lines
  validate minutes
  total by topic
  print report
```

Possible split:

```text
main.rs       args, file read, print
parser.rs     parse lines
model.rs      StudyEntry
report.rs     total by topic, render report
error.rs      ParseError
```

---

## Cleanliness Checklist

Before a Rust project grows, ask:

```text
Can I explain each module in one sentence?
Are invalid states protected by types?
Is the public API smaller than the internal file tree?
Can I test core rules without file/network/terminal I/O?
Does main.rs mostly orchestrate?
Are errors represented clearly?
Are modules named after real concepts?
Can a new reader find the entry point?
```

If the answer is no, refactor before adding the next feature.

---

## Mini Project: Redesign A Study Log

Given this bad layout:

```text
src/
  main.rs
  utils.rs
```

And `utils.rs` contains:

```text
parse_entry
parse_minutes
print_summary
read_file
StudyEntry
validate_topic
```

Propose a better layout:

```text
src/
  lib.rs
  main.rs
  model.rs
  parser.rs
  report.rs
  error.rs
```

Suggested responsibilities:

```text
model.rs
  StudyEntry

parser.rs
  parse_entry
  parse_entries
  parse_minutes
  validate_topic

report.rs
  render_summary

error.rs
  ParseError

main.rs
  read_file
  print_summary
```

`lib.rs` re-exports the public API:

```rust
mod error;
mod model;
mod parser;
mod report;

pub use error::ParseError;
pub use model::StudyEntry;
pub use parser::{parse_entries, parse_entry};
pub use report::render_summary;
```

---

## Chapter Checkpoint

You should now be able to answer:

- What makes a module boundary clean?
- Why is `utils.rs` often a smell?
- Why should public API be smaller than file structure?
- How do you break parser/report circular pressure?
- Why keep I/O at program edges?
- When should you split a module?
- Where should tests live for private helpers?

---

## Recap

- Clean Rust architecture is about clear responsibility.
- Keep internals private and public behavior intentional.
- Use domain names for modules.
- Extract shared model types to break circular pressure.
- Keep side effects near binary edges.
- Refactor by moving one responsibility at a time.

## Try It Yourself

Take a previous project and draw its module map. Then write:

```text
Public API:
Private modules:
Where I/O happens:
Where validation happens:
Where errors are defined:
One boundary I should improve:
```

---

[**Next ->** Project Tutorial: Inventory CLI With A Reusable Library](./06-chapter-05-project-tutorials.md)  
[**<- Previous** Binary Crates: Applications And Entry Points](./04-binary-crates-applications-and-entry-points.md)
