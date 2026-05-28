<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 06](./README.md)

---

# Files, CLI Input, Environment, and Useful Programs

> A learner starts writing software when their program accepts input, reads files, handles errors, and produces useful output.

**You will learn:**
- how command-line arguments work
- how to read and write files
- how to parse line-based data
- how to use environment variables for configuration
- how to avoid panic-driven beginner programs
- how to build a small useful report tool

**Before this page, you should know:** [Traits, Generics, Iterators, and Closures](./03-traits-generics-iterators-and-closures.md)

---

## Command-Line Arguments

Command-line arguments are text values passed after the program name.

```bash
cargo run -- report study-log.txt
```

Inside Rust:

```rust
use std::env;

fn main() {
    for arg in env::args() {
        println!("{arg}");
    }
}
```

The first argument is usually the program path. The values after it are the
actual user-provided arguments.

For a small beginner CLI, collect them:

```rust
use std::env;

#[derive(Debug, PartialEq, Eq)]
enum Command {
    Report { path: String },
    Help,
}

fn parse_args(args: impl IntoIterator<Item = String>) -> Command {
    let mut args = args.into_iter();
    let _program = args.next();

    match (args.next().as_deref(), args.next()) {
        (Some("report"), Some(path)) => Command::Report { path },
        _ => Command::Help,
    }
}
```

This keeps argument parsing testable because `parse_args` accepts any iterator of
strings, not only real process arguments.

## File Reading

```rust
use std::fs;
use std::io;

fn read_text_file(path: &str) -> io::Result<String> {
    fs::read_to_string(path)
}
```

`io::Result<String>` is an alias for `Result<String, std::io::Error>`.

Use it:

```rust
let contents = read_text_file("study-log.txt")?;
```

Reading a file can fail because:

- the file does not exist
- the program lacks permission
- the path points to a directory
- the bytes are not valid UTF-8 for `read_to_string`

That is why this returns `Result`.

## File Writing

```rust
use std::fs;
use std::io;

fn write_text_file(path: &str, contents: &str) -> io::Result<()> {
    fs::write(path, contents)
}
```

`()` means success has no extra value. The meaningful part is whether writing
failed.

## Parse a Line-Based Format

Use a beginner-friendly text format:

```text
Rust ownership,45
Cargo projects,30
Borrowing practice,25
```

Parser:

```rust
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
```

`splitn(2, ',')` splits at the first comma only. That is safer than unlimited
splitting if a topic might contain punctuation later.

## Parse a Whole File

```rust
pub fn parse_entries(contents: &str) -> Vec<Result<StudyEntry, ParseEntryError>> {
    contents
        .lines()
        .filter(|line| !line.trim().is_empty())
        .map(parse_entry_line)
        .collect()
}
```

This returns a vector of results so one bad line does not erase every good line.

For strict parsing, fail on the first bad line:

```rust
pub fn parse_entries_strict(contents: &str) -> Result<Vec<StudyEntry>, ParseEntryError> {
    contents
        .lines()
        .filter(|line| !line.trim().is_empty())
        .map(parse_entry_line)
        .collect()
}
```

Rust can collect an iterator of `Result<T, E>` into `Result<Vec<T>, E>`.

## Environment Variables

Environment variables configure programs from outside the code.

```rust
use std::env;

fn default_log_path() -> String {
    env::var("STUDY_LOG_PATH").unwrap_or_else(|_| String::from("study-log.txt"))
}
```

Use env vars for machine-specific configuration. Do not use them as a substitute
for clear command-line arguments when the user should choose a value directly.

## Main Without Panic Soup

Beginner Rust often becomes `.unwrap()` everywhere. Replace that with a small
`run` function returning `Result`.

```rust
use std::error::Error;

fn main() {
    if let Err(error) = run() {
        eprintln!("error: {error}");
        std::process::exit(1);
    }
}

fn run() -> Result<(), Box<dyn Error>> {
    let command = parse_args(std::env::args());

    match command {
        Command::Report { path } => {
            let contents = std::fs::read_to_string(path)?;
            let entries = parse_entries_strict(&contents)?;
            println!("{}", format_report(&entries));
        }
        Command::Help => {
            println!("Usage: study-log report <path>");
        }
    }

    Ok(())
}
```

`Box<dyn Error>` means "some error type that implements the Error trait." It is
acceptable for small application entry points. Libraries should usually return
more specific error types.

## Useful Program Shape

```text
main.rs
    |
    v
parse command
    |
    v
read file
    |
    v
parse entries
    |
    v
format report
    |
    v
print output
```

Each step can be a separate function. Most can be tested without running the
whole program.

## Reference Links

- [Cargo Commands](../../reference/cargo-commands.md)
- [Errors, Warnings, and Debugging](../../reference/errors-warnings-and-debugging.md)
- [Types, Strings, and Collections](../../reference/types-strings-and-collections.md)

---

## Recap

- CLI arguments are strings from the process.
- File operations return `Result` because files fail in normal life.
- Parse text at the boundary and turn it into typed data.
- Environment variables are useful for configuration.
- Put fallible app logic in `run() -> Result<...>` and keep `main` small.

## Try It Yourself

Build `study-log report study-log.txt`. The file should contain `topic,minutes`
lines. Print total minutes and totals by topic. Invalid lines should produce a
clear error instead of a panic.

---

[**Next ->** Macros, Attributes, Docs, Features, and Polish](./05-macros-attributes-docs-features-and-polish.md)  
[**<- Previous** Traits, Generics, Iterators, and Closures](./03-traits-generics-iterators-and-closures.md)
