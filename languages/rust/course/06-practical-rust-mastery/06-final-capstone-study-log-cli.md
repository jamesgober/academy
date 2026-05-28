<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 06](./README.md)

---

# Final Capstone: Study Log CLI

> Build a real Rust command-line app with modules, validation, files, errors, tests, docs, and a clean user workflow.

This capstone is the "I can write software" project for the Rust track. It is
still small, but it has the bones of real Rust: domain types, fallible parsing,
file I/O, command parsing, formatting, tests, and clear module boundaries.

## Final Structure

```text
study-log/
|-- Cargo.toml
|-- README.md
|-- src/
|   |-- main.rs
|   |-- lib.rs
|   |-- entry.rs
|   |-- parser.rs
|   |-- report.rs
|   `-- cli.rs
|-- tests/
|   `-- cli_flow.rs
`-- examples/
    `-- sample-log.txt
```

## Step 1: Create the Project

```bash
cargo new study-log
cd study-log
```

`Cargo.toml`:

```toml
[package]
name = "study-log"
version = "0.1.0"
edition = "2024"
description = "A beginner-friendly Rust CLI for summarizing study sessions."
license = "MIT OR Apache-2.0"

[dependencies]
```

## Step 2: Library Root

`src/lib.rs`:

```rust
pub mod cli;
pub mod entry;
pub mod parser;
pub mod report;

pub use cli::{parse_args, Command};
pub use entry::{EntryError, StudyEntry};
pub use parser::{parse_entries_strict, parse_entry_line, ParseEntryError};
pub use report::{format_report, total_minutes, totals_by_topic};
```

This is the public API. `main.rs` and integration tests can use these exports.

## Step 3: Entry Model

`src/entry.rs`:

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

## Step 4: Parser

`src/parser.rs`:

```rust
use crate::entry::{EntryError, StudyEntry};

#[derive(Debug, Clone, PartialEq, Eq)]
pub enum ParseEntryError {
    MissingTopic,
    MissingMinutes,
    InvalidMinutes,
}

pub fn parse_entry_line(line: &str) -> Result<StudyEntry, ParseEntryError> {
    let mut parts = line.splitn(2, ',');
    let topic = parts.next().ok_or(ParseEntryError::MissingTopic)?;
    let minutes_raw = parts.next().ok_or(ParseEntryError::MissingMinutes)?;

    let minutes = minutes_raw
        .trim()
        .parse::<u32>()
        .map_err(|_| ParseEntryError::InvalidMinutes)?;

    StudyEntry::new(topic, minutes).map_err(|error| match error {
        EntryError::EmptyTopic => ParseEntryError::MissingTopic,
        EntryError::InvalidMinutes => ParseEntryError::InvalidMinutes,
    })
}

pub fn parse_entries_strict(contents: &str) -> Result<Vec<StudyEntry>, ParseEntryError> {
    contents
        .lines()
        .filter(|line| !line.trim().is_empty())
        .map(parse_entry_line)
        .collect()
}
```

## Step 5: Report

`src/report.rs`:

```rust
use std::collections::HashMap;

use crate::entry::StudyEntry;

pub fn total_minutes(entries: &[StudyEntry]) -> u32 {
    entries.iter().map(StudyEntry::minutes).sum()
}

pub fn totals_by_topic(entries: &[StudyEntry]) -> HashMap<String, u32> {
    let mut totals = HashMap::new();

    for entry in entries {
        *totals.entry(entry.topic().to_owned()).or_insert(0) += entry.minutes();
    }

    totals
}

pub fn format_report(entries: &[StudyEntry]) -> String {
    let total = total_minutes(entries);
    let mut lines = vec![format!("Total study time: {total} minutes")];

    let mut by_topic = totals_by_topic(entries)
        .into_iter()
        .collect::<Vec<_>>();

    by_topic.sort_by(|left, right| left.0.cmp(&right.0));

    for (topic, minutes) in by_topic {
        lines.push(format!("- {topic}: {minutes} minutes"));
    }

    lines.join("\n")
}
```

## Step 6: CLI Parsing

`src/cli.rs`:

```rust
#[derive(Debug, Clone, PartialEq, Eq)]
pub enum Command {
    Report { path: String },
    Help,
}

pub fn parse_args(args: impl IntoIterator<Item = String>) -> Command {
    let mut args = args.into_iter();
    let _program = args.next();

    match (args.next().as_deref(), args.next()) {
        (Some("report"), Some(path)) => Command::Report { path },
        _ => Command::Help,
    }
}
```

This is deliberately small. In larger apps, you would use a CLI crate such as
`clap`, but hand-writing this once teaches what the crate later automates.

## Step 7: App Entry

`src/main.rs`:

```rust
use std::error::Error;

use study_log::{format_report, parse_args, parse_entries_strict, Command};

fn main() {
    if let Err(error) = run() {
        eprintln!("error: {error:?}");
        std::process::exit(1);
    }
}

fn run() -> Result<(), Box<dyn Error>> {
    match parse_args(std::env::args()) {
        Command::Report { path } => {
            let contents = std::fs::read_to_string(path)?;
            let entries = parse_entries_strict(&contents)
                .map_err(|error| format!("could not parse study log: {error:?}"))?;

            println!("{}", format_report(&entries));
        }
        Command::Help => {
            println!("Usage:");
            println!("  study-log report <path>");
        }
    }

    Ok(())
}
```

`main` is tiny. `run` handles fallible application flow. Domain logic lives in
the library.

## Step 8: Sample Data

`examples/sample-log.txt`:

```text
Rust ownership,45
Borrowing practice,30
Cargo projects,25
Rust ownership,15
```

Run:

```bash
cargo run -- report examples/sample-log.txt
```

Expected output:

```text
Total study time: 115 minutes
- Borrowing practice: 30 minutes
- Cargo projects: 25 minutes
- Rust ownership: 60 minutes
```

## Step 9: Tests

Inside each module, add focused unit tests. Example in `parser.rs`:

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn parses_valid_line() {
        let entry = parse_entry_line("Rust,45").unwrap();

        assert_eq!(entry.topic(), "Rust");
        assert_eq!(entry.minutes(), 45);
    }

    #[test]
    fn rejects_missing_minutes() {
        let error = parse_entry_line("Rust").unwrap_err();

        assert_eq!(error, ParseEntryError::MissingMinutes);
    }
}
```

Integration test in `tests/cli_flow.rs`:

```rust
use study_log::{format_report, parse_entries_strict};

#[test]
fn formats_report_from_text() {
    let input = "Rust,45\nCargo,15\nRust,15\n";
    let entries = parse_entries_strict(input).unwrap();
    let report = format_report(&entries);

    assert!(report.contains("Total study time: 75 minutes"));
    assert!(report.contains("- Cargo: 15 minutes"));
    assert!(report.contains("- Rust: 60 minutes"));
}
```

Run:

```bash
cargo test
```

## Step 10: README

`README.md`:

```markdown
# Study Log

A beginner-friendly Rust CLI for summarizing study sessions.

## Input Format

Each non-empty line uses:

```text
topic,minutes
```

## Run

```bash
cargo run -- report examples/sample-log.txt
```

## Test

```bash
cargo fmt --check
cargo clippy --all-targets -- -D warnings
cargo test
```
```

## Final Quality Loop

```bash
cargo fmt --check
cargo clippy --all-targets -- -D warnings
cargo test
cargo run -- report examples/sample-log.txt
```

## What You Built

You built a real Rust program:

```text
CLI args -> file read -> parsing -> typed domain data -> report formatting -> output
```

That flow is the skeleton of many command-line tools, importers, data processors,
and automation scripts.

---

## Recap

- Real Rust programs are built from small, honest modules.
- Domain types protect valid data.
- Parsing turns messy text into typed values.
- Reports are data transformations.
- `main` should coordinate, not contain all logic.
- Tests make the project safer to change.

## Try It Yourself

Add a second command:

```bash
cargo run -- add study-log.txt "Rust iterators" 40
```

It should append a validated entry to the file. If the topic or minutes are
invalid, it should print a useful error and leave the file unchanged.

---

[**Next ->** Rust Reference](../../reference/README.md)  
[**<- Previous** Macros, Attributes, Docs, Features, and Polish](./05-macros-attributes-docs-features-and-polish.md)
