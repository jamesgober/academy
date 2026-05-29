<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 02](./README.md)

---

# Structs, Enums, and Pattern Matching

> Rust becomes easier when you model the real shape of your data instead of juggling loose values.

**You will learn:**
- how structs group related fields
- how methods and constructors work through `impl`
- how enums represent one-of-many states
- how `match`, `if let`, and destructuring reveal data safely
- how to model invalid states out of existence

**Before this page, you should know:** [Lifetimes in Plain Language](./04-lifetimes-in-plain-language.md)

---

## Structs Group Data

A struct is a custom type with named fields.

```rust
#[derive(Debug, Clone, PartialEq, Eq)]
struct StudyEntry {
    topic: String,
    minutes: u32,
}
```

Create one:

```rust
fn main() {
    let entry = StudyEntry {
        topic: String::from("Rust structs"),
        minutes: 45,
    };

    println!("{entry:?}");
}
```

`#[derive(Debug)]` lets you print with `{:?}`. `derive` asks Rust to generate
standard trait implementations when the automatic behavior is correct.

## Methods with `impl`

Methods are functions attached to a type.

```rust
impl StudyEntry {
    fn new(topic: impl Into<String>, minutes: u32) -> Self {
        Self {
            topic: topic.into(),
            minutes,
        }
    }

    fn topic(&self) -> &str {
        &self.topic
    }

    fn minutes(&self) -> u32 {
        self.minutes
    }
}
```

`Self` means the current type, `StudyEntry`.

Use it:

```rust
fn main() {
    let entry = StudyEntry::new("Rust methods", 30);

    println!("{}: {}", entry.topic(), entry.minutes());
}
```

`StudyEntry::new` is an associated function because it does not take `self`.
`entry.topic()` is a method because it takes `&self`.

## Private Fields and Public Methods

In a module, fields are private by default. Public APIs often expose methods
instead of fields.

```rust
pub struct StudyEntry {
    topic: String,
    minutes: u32,
}

impl StudyEntry {
    pub fn new(topic: impl Into<String>, minutes: u32) -> Self {
        Self {
            topic: topic.into(),
            minutes,
        }
    }

    pub fn topic(&self) -> &str {
        &self.topic
    }
}
```

Private fields let the type protect its own rules. Later you can add validation
without letting callers build nonsense values directly.

## Tuple Structs and Unit Structs

Tuple struct:

```rust
struct Minutes(u32);
```

Use it when one value needs a stronger meaning than its raw type.

```rust
let session = Minutes(45);
```

Unit struct:

```rust
struct NoopLogger;
```

Unit structs are useful for marker types or types whose behavior matters more
than stored data.

## Enums Represent One Valid State

An enum value is exactly one variant at a time.

```rust
#[derive(Debug, Clone, PartialEq, Eq)]
enum SessionStatus {
    Planned,
    InProgress { started_at: String },
    Completed { minutes: u32 },
    Rejected { reason: String },
}
```

This is stronger than several booleans:

```rust
struct BadStatus {
    planned: bool,
    in_progress: bool,
    completed: bool,
}
```

With booleans, a value could be both planned and completed by accident. With an
enum, the session is exactly one state.

## Pattern Matching with `match`

```rust
fn describe_status(status: &SessionStatus) -> String {
    match status {
        SessionStatus::Planned => String::from("planned"),
        SessionStatus::InProgress { started_at } => {
            format!("started at {started_at}")
        }
        SessionStatus::Completed { minutes } => {
            format!("completed after {minutes} minutes")
        }
        SessionStatus::Rejected { reason } => {
            format!("rejected: {reason}")
        }
    }
}
```

`match` is exhaustive. Rust forces you to handle every variant.

That is a gift. If you add a new enum variant later, Rust points to every place
that must decide what the new state means.

## `if let` for One Interesting Variant

Use `if let` when you only care about one pattern.

```rust
fn print_completion(status: &SessionStatus) {
    if let SessionStatus::Completed { minutes } = status {
        println!("completed in {minutes} minutes");
    }
}
```

Use `match` when every variant matters. Use `if let` when one variant matters
and the rest can be ignored.

## Real Model: Intake Result

```rust
#[derive(Debug, Clone, PartialEq, Eq)]
struct IntakeRequest {
    topic: String,
    minutes_raw: String,
}

#[derive(Debug, Clone, PartialEq, Eq)]
enum IntakeStatus {
    Accepted(StudyEntry),
    Rejected { topic: String, reason: String },
}

fn process_request(request: IntakeRequest) -> IntakeStatus {
    let topic = request.topic.trim();

    if topic.is_empty() {
        return IntakeStatus::Rejected {
            topic: request.topic,
            reason: String::from("topic is empty"),
        };
    }

    let minutes = match request.minutes_raw.trim().parse::<u32>() {
        Ok(minutes) => minutes,
        Err(_) => {
            return IntakeStatus::Rejected {
                topic: topic.to_owned(),
                reason: String::from("minutes must be a number"),
            };
        }
    };

    if minutes == 0 {
        return IntakeStatus::Rejected {
            topic: topic.to_owned(),
            reason: String::from("minutes must be greater than zero"),
        };
    }

    IntakeStatus::Accepted(StudyEntry::new(topic, minutes))
}
```

This code models the business reality:

- raw input is not trusted
- accepted data becomes a `StudyEntry`
- rejected data carries a reason
- callers cannot confuse accepted and rejected states

## Render Status Messages

```rust
fn format_status(status: &IntakeStatus) -> String {
    match status {
        IntakeStatus::Accepted(entry) => {
            format!("accepted: {} for {} minutes", entry.topic(), entry.minutes())
        }
        IntakeStatus::Rejected { topic, reason } => {
            format!("rejected `{topic}`: {reason}")
        }
    }
}
```

## Reference Links

- [Types, Strings, and Collections](../../reference/types-strings-and-collections.md)
- [Functions, Methods, Generics, and Traits](../../reference/functions-methods-generics-and-traits.md)
- [Errors, Warnings, and Debugging](../../reference/errors-warnings-and-debugging.md)

---

## Recap

- Structs group related fields into a named type.
- `impl` blocks define methods and associated functions.
- Private fields plus public methods protect invariants.
- Enums model exactly one valid state.
- `match` forces every enum variant to be handled.
- `if let` is useful when only one variant matters.

## Try It Yourself

Model a download system:

- `DownloadRequest { url: String }`
- `DownloadStatus::Queued`
- `DownloadStatus::Running { percent: u8 }`
- `DownloadStatus::Finished { path: String }`
- `DownloadStatus::Failed { reason: String }`

Write `format_download_status(&DownloadStatus) -> String`.

---

[**Next ->** Error Handling with Option and Result](./06-error-handling-with-option-and-result.md)  
[**<- Previous** Lifetimes in Plain Language](./04-lifetimes-in-plain-language.md)
