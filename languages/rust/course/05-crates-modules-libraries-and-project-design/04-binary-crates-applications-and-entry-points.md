<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 05](./README.md)

---

# Binary Crates: Applications And Entry Points

> A binary crate is the executable part of your Rust project. Its job is to wire
> the outside world to your library, not become a junk drawer for all logic.

**You will learn:**
- What a binary crate is
- What belongs in `main`
- How to return clean errors from application code
- How to use multiple binaries in one package
- How to keep CLI code testable by moving decisions into library functions

**Before this page, you should know:** [Library Crates: When And How To Build Them](./03-library-crates-when-and-how-to-build-them.md)

---

## Binary Crate In Plain Language

A binary crate builds an executable program.

Default binary:

```text
src/main.rs
```

Extra binaries:

```text
src/bin/import.rs
src/bin/export.rs
```

Run:

```bash
cargo run
cargo run --bin import
cargo run --bin export
```

---

## What `main` Should Do

Good `main` responsibilities:

- Read command-line args
- Read environment variables
- Call library code
- Print success output
- Print error output
- Choose exit behavior

Bad `main` responsibilities:

- Contain all parsing rules
- Contain all validation rules
- Contain all business logic
- Contain a pile of unrelated helper functions
- Be impossible to test without running the whole program

Visual model:

```text
main.rs
  outside world adapter
      |
      v
library crate
  real rules and behavior
```

---

## Thin `main` Pattern

`src/main.rs`:

```rust
use std::process::ExitCode;

fn main() -> ExitCode {
    match run() {
        Ok(()) => ExitCode::SUCCESS,
        Err(error) => {
            eprintln!("error: {error}");
            ExitCode::FAILURE
        }
    }
}

fn run() -> Result<(), String> {
    let args: Vec<String> = std::env::args().collect();
    let name = args.get(1).ok_or("missing name argument")?;

    let message = greeter::greeting(name);
    println!("{message}");

    Ok(())
}
```

`src/lib.rs`:

```rust
pub fn greeting(name: &str) -> String {
    format!("Hello, {name}")
}
```

This is already better than putting everything in `main`.

Why?

- `greeting` is easy to test.
- `run` handles application flow.
- `main` handles process exit.

---

## `main` Can Return `Result`

Rust also allows:

```rust
fn main() -> Result<(), Box<dyn std::error::Error>> {
    let text = std::fs::read_to_string("input.txt")?;
    println!("{text}");

    Ok(())
}
```

This is convenient for small tools.

Tradeoff:

- Good: short, uses `?`
- Less control: default error printing may not match your desired CLI experience

For beginner learning projects, either style is okay. For polished CLI tools,
the `run()` plus `ExitCode` pattern gives more control.

---

## Parse Arguments Without Making A Mess

Start simple:

```rust
pub struct Config {
    pub topic: String,
    pub minutes: u32,
}

pub fn parse_args(args: &[String]) -> Result<Config, String> {
    let topic = args
        .get(1)
        .ok_or("usage: study-log <topic> <minutes>")?
        .to_string();

    let minutes_text = args
        .get(2)
        .ok_or("usage: study-log <topic> <minutes>")?;

    let minutes = minutes_text
        .parse::<u32>()
        .map_err(|_| format!("minutes must be a number, got {minutes_text}"))?;

    Ok(Config { topic, minutes })
}
```

Now test argument parsing without launching the binary:

```rust
#[test]
fn parses_topic_and_minutes() {
    let args = vec![
        "study-log".to_string(),
        "Rust".to_string(),
        "45".to_string(),
    ];

    let config = parse_args(&args).unwrap();

    assert_eq!(config.topic, "Rust");
    assert_eq!(config.minutes, 45);
}
```

Later, serious CLI projects often use crates such as `clap`, but understanding
the manual version first makes those crates easier to appreciate.

---

## Multiple Binaries Sharing One Library

Project:

```text
study-tools/
  Cargo.toml
  src/
    lib.rs
    bin/
      add_session.rs
      summarize.rs
```

`src/lib.rs`:

```rust
pub fn normalize_topic(topic: &str) -> String {
    topic.trim().to_lowercase()
}
```

`src/bin/add_session.rs`:

```rust
use study_tools::normalize_topic;

fn main() {
    let topic = normalize_topic(" Rust ");
    println!("adding session for {topic}");
}
```

`src/bin/summarize.rs`:

```rust
use study_tools::normalize_topic;

fn main() {
    let topic = normalize_topic(" RUST ");
    println!("summarizing sessions for {topic}");
}
```

Run:

```bash
cargo run --bin add_session
cargo run --bin summarize
```

Both binaries use the same library behavior.

---

## Where To Put Side Effects

Side effects include:

- Printing
- Reading files
- Writing files
- Reading environment variables
- Making network calls
- Exiting the process

Keep side effects near the edges of the program.

Example:

```rust
// Library: pure and testable
pub fn render_summary(topic: &str, minutes: u32) -> String {
    format!("{topic}: {minutes} minutes")
}
```

```rust
// Binary: does I/O
fn main() {
    let summary = study_tools::render_summary("Rust", 45);
    println!("{summary}");
}
```

If `render_summary` printed directly, testing would be harder and reuse would be
worse.

---

## Exit Codes

Programs communicate success or failure to shells using exit codes.

```text
0     success
non-0 failure
```

Rust:

```rust
use std::process::ExitCode;

fn main() -> ExitCode {
    if let Err(error) = run() {
        eprintln!("error: {error}");
        return ExitCode::FAILURE;
    }

    ExitCode::SUCCESS
}

fn run() -> Result<(), String> {
    Ok(())
}
```

This matters for scripts and CI:

```bash
cargo run --bin import
echo $?
```

On PowerShell:

```powershell
cargo run --bin import
$LASTEXITCODE
```

---

## Mini Project: Two Commands, One Library

Create:

```text
study-tools/
  src/
    lib.rs
    bin/
      add.rs
      summary.rs
```

`src/lib.rs`:

```rust
pub fn normalize_topic(topic: &str) -> Result<String, String> {
    let topic = topic.trim();

    if topic.is_empty() {
        return Err("topic cannot be empty".to_string());
    }

    Ok(topic.to_lowercase())
}

pub fn render_session(topic: &str, minutes: u32) -> String {
    format!("{topic}: {minutes} minutes")
}
```

Test the library:

```rust
#[test]
fn rejects_empty_topic() {
    assert_eq!(normalize_topic(" "), Err("topic cannot be empty".to_string()));
}

#[test]
fn renders_session() {
    assert_eq!(render_session("rust", 45), "rust: 45 minutes");
}
```

Then make both binaries call the library. Keep parsing and printing in binaries.

---

## Chapter Checkpoint

You should now be able to answer:

- What file creates the default binary crate?
- What does `src/bin/*.rs` do?
- What should `main` be responsible for?
- Why is `run() -> Result<..., ...>` a useful pattern?
- Why should side effects stay near program edges?
- How can argument parsing be made testable?
- What does a non-zero exit code mean?

---

## Recap

- Binary crates create executable programs.
- Keep `main` thin.
- Put reusable rules in the library crate.
- Put printing, environment, files, and process exit at the application edge.
- Multiple binaries can share one library.

## Try It Yourself

Refactor one previous program so:

- `main.rs` only calls `run`
- `run` parses input and calls the library
- `lib.rs` owns the rules
- At least one parsing helper has tests

---

[**Next ->** Modular Design And Codebase Cleanliness](./05-modular-design-and-codebase-cleanliness.md)  
[**<- Previous** Library Crates: When And How To Build Them](./03-library-crates-when-and-how-to-build-them.md)
