<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 03](./README.md)

---

# Debugging Test Failures And Flaky Behavior

> A failing test is not the enemy. A confusing failing test is the enemy.

**You will learn:**
- How to read a Rust test failure
- How to run one test with printed output
- How to use `dbg!`, `println!`, and backtraces without making a mess
- How to shrink a failure to the smallest useful case
- Why flaky tests happen and how to make them deterministic

**Before this page, you should know:** [Rust CI Workflows with GitHub Actions](./04-rust-ci-workflows-with-github-actions.md)

---

## The Debugging Loop

When a test fails, do this:

```text
1. Reproduce it locally.
2. Run only the failing test.
3. Read the first useful failure message.
4. Print or inspect the values involved.
5. Shrink the input.
6. Fix the code or fix the test.
7. Run the full suite.
```

Do not start by changing random code. First make the failure small enough to
understand.

---

## A Failing Test Example

Code:

```rust
pub fn average_minutes(minutes: &[u32]) -> u32 {
    let total: u32 = minutes.iter().sum();
    total / minutes.len() as u32
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn empty_average_is_zero() {
        assert_eq!(average_minutes(&[]), 0);
    }
}
```

Run:

```bash
cargo test
```

Failure:

```text
thread 'tests::empty_average_is_zero' panicked at src/lib.rs:3:5:
attempt to divide by zero
```

The test did its job. It found an edge case.

Fix:

```rust
pub fn average_minutes(minutes: &[u32]) -> u32 {
    if minutes.is_empty() {
        return 0;
    }

    let total: u32 = minutes.iter().sum();
    total / minutes.len() as u32
}
```

Lesson:

> A failing test should point to one specific broken expectation.

---

## Run One Test

When a suite gets large, run one test by name:

```bash
cargo test empty_average_is_zero
```

Rust treats the name as a filter. You can also filter by part of a module path:

```bash
cargo test average
```

If multiple tests contain `average`, they all run.

---

## See Printed Output

Rust hides successful test output by default.

Use `-- --nocapture`:

```bash
cargo test empty_average_is_zero -- --nocapture
```

Example:

```rust
#[test]
fn parses_minutes() {
    let input = "Rust|45|ownership";
    let parsed = parse_entry(input);

    println!("parsed = {parsed:?}");

    assert!(parsed.is_ok());
}
```

`-- --nocapture` means:

```text
cargo test arguments        test binary arguments
--------------------        ---------------------
cargo test name             --nocapture
```

The first `--` separates Cargo's arguments from the test runner's arguments.

---

## Use `dbg!` For Quick Inspection

`dbg!` prints the file, line, expression, and value. It also returns the value,
so it can be inserted into expressions.

```rust
#[test]
fn totals_rust_minutes() {
    let entries = vec![
        StudyEntry::new("Rust", 45, "ownership").unwrap(),
        StudyEntry::new("Git", 20, "commits").unwrap(),
    ];

    let total = dbg!(total_minutes_for_topic(&entries, "Rust"));

    assert_eq!(total, 45);
}
```

Example output:

```text
[src/lib.rs:42:17] total_minutes_for_topic(&entries, "Rust") = 45
```

Remove `dbg!` before committing unless the output is intentionally part of the
program.

---

## Read `assert_eq!` Correctly

Rust prints left and right values:

```rust
assert_eq!(actual, expected);
```

Failure:

```text
assertion `left == right` failed
  left: 40
 right: 45
```

In most Rust code, the convention is:

```rust
assert_eq!(actual, expected);
```

That makes the message read:

```text
left: what the code produced
right: what the test expected
```

Some teams reverse the convention. Be consistent inside a project.

---

## Enable Backtraces

When a panic is buried under several function calls, a backtrace shows how Rust
got there.

PowerShell:

```powershell
$env:RUST_BACKTRACE = "1"
cargo test empty_average_is_zero
```

Bash:

```bash
RUST_BACKTRACE=1 cargo test empty_average_is_zero
```

Use a backtrace when:

- The panic location is not enough
- A helper function panicked
- You need to see the call path

Backtraces are noisy at first. Look for your files, not Rust internals.

---

## Shrink The Failure

Big failing input is hard to understand.

Imagine this test fails:

```rust
#[test]
fn parses_export_file() {
    let input = include_str!("../fixtures/export.txt");
    let entries = parse_export(input).unwrap();

    assert_eq!(entries.len(), 1200);
}
```

That gives you a huge search space.

Shrink it:

```rust
#[test]
fn rejects_missing_minutes_field() {
    let input = "Rust||ownership";
    let error = parse_entry(input).unwrap_err();

    assert_eq!(error.to_string(), "minutes is required");
}
```

Now the failure is about one rule.

Shrinking questions:

- Can I reproduce it with one line instead of a whole file?
- Can I remove fields that are not part of the problem?
- Can I replace real data with tiny fake data?
- Can I write the expected result directly?

---

## Fix Code Or Fix Test?

Sometimes the code is wrong. Sometimes the test is wrong.

Ask:

```text
What behavior should the user reasonably expect?
What did the public API promise?
Does the test match the lesson, README, or reference docs?
Is the test relying on an implementation detail?
Is the test brittle for no useful reason?
```

Bad test:

```rust
#[test]
fn stores_entries_in_exact_internal_order() {
    let log = StudyLog::from_entries(sample_entries());

    assert_eq!(log.internal_hash_map_keys(), vec!["Git", "Rust"]);
}
```

If the API never promised internal map key order, this test is fragile.

Better test:

```rust
#[test]
fn can_find_entries_by_topic() {
    let log = StudyLog::from_entries(sample_entries());

    assert_eq!(log.total_minutes_for_topic("Rust"), 75);
    assert_eq!(log.total_minutes_for_topic("Git"), 20);
}
```

Test public behavior when possible.

---

## What Flaky Means

A flaky test sometimes passes and sometimes fails without code changes.

That is dangerous because people stop trusting the test suite.

Common causes:

| Cause | Example | Fix |
|---|---|---|
| Wall-clock time | Test assumes current date | Inject a clock or use fixed time |
| Randomness | Uses random values without a seed | Use fixed seed or explicit cases |
| Test order | Test B depends on Test A running first | Make every test independent |
| Shared files | Tests write to same file path | Use unique temp paths |
| Shared global state | One test changes process-wide config | Reset state or avoid globals |
| Network | Test calls real API | Use fake response |
| Parallel execution | Tests mutate same resource at same time | Isolate resources or limit threads |
| Environment variables | Test depends on local env var | Set env inside the test |

---

## Parallel Tests

Rust runs tests in parallel by default.

That is usually good. It makes tests fast.

But tests can collide if they share mutable external state:

```rust
#[test]
fn writes_report_a() {
    std::fs::write("report.txt", "A").unwrap();
    assert_eq!(std::fs::read_to_string("report.txt").unwrap(), "A");
}

#[test]
fn writes_report_b() {
    std::fs::write("report.txt", "B").unwrap();
    assert_eq!(std::fs::read_to_string("report.txt").unwrap(), "B");
}
```

These tests fight over `report.txt`.

Better:

```rust
fn unique_test_file(name: &str) -> std::path::PathBuf {
    let mut path = std::env::temp_dir();
    path.push(format!("{name}-{}", std::process::id()));
    path
}

#[test]
fn writes_report_a() {
    let path = unique_test_file("report-a");

    std::fs::write(&path, "A").unwrap();
    assert_eq!(std::fs::read_to_string(&path).unwrap(), "A");

    let _ = std::fs::remove_file(path);
}
```

If you suspect a parallelism problem, run:

```bash
cargo test -- --test-threads=1
```

If the failure disappears, tests are probably sharing state.

Do not leave the suite dependent on single-threaded execution unless you have a
clear reason. Fix the shared state.

---

## Environment Variables In Tests

Environment variables are process-wide. If one test changes an environment
variable, another test can see the change.

Risky:

```rust
#[test]
fn reads_mode_from_env() {
    std::env::set_var("APP_MODE", "test");

    assert_eq!(read_mode(), "test");
}
```

Better design:

```rust
pub fn mode_from_value(value: Option<&str>) -> String {
    value.unwrap_or("development").to_string()
}

pub fn read_mode() -> String {
    mode_from_value(std::env::var("APP_MODE").ok().as_deref())
}

#[test]
fn defaults_to_development_when_mode_is_missing() {
    assert_eq!(mode_from_value(None), "development");
}

#[test]
fn uses_provided_mode_value() {
    assert_eq!(mode_from_value(Some("test")), "test");
}
```

The pure helper is easy to test. The environment-reading wrapper stays tiny.

---

## Network Tests

Tests that call real services are slow, flaky, and sometimes expensive.

Instead of this:

```rust
#[test]
fn fetches_real_profile() {
    let profile = fetch_profile_from_api("https://example.com/users/1").unwrap();

    assert_eq!(profile.name, "Ada");
}
```

Separate the network from the parsing:

```rust
#[derive(Debug, PartialEq, Eq)]
pub struct Profile {
    pub name: String,
}

pub fn parse_profile_json(json: &str) -> Result<Profile, String> {
    if json.contains("\"name\":\"Ada\"") {
        Ok(Profile {
            name: "Ada".to_string(),
        })
    } else {
        Err("missing name".to_string())
    }
}

#[test]
fn parses_profile_name() {
    let json = r#"{"name":"Ada"}"#;

    assert_eq!(
        parse_profile_json(json),
        Ok(Profile {
            name: "Ada".to_string()
        })
    );
}
```

This example uses a deliberately tiny parser so the testing idea stays visible.
In real JSON code, you would usually use `serde_json`.

---

## Failure Triage Checklist

When a test fails, write down:

```text
Test name:
Command used:
First error:
Smallest failing input:
Expected behavior:
Actual behavior:
Likely cause:
Fix:
Full suite passed after fix:
```

This turns debugging from panic into bookkeeping.

---

## Mini Project: Fix A Flaky File Test

Start with this bad test pair:

```rust
#[test]
fn saves_first_report() {
    std::fs::write("report.txt", "first").unwrap();

    assert_eq!(std::fs::read_to_string("report.txt").unwrap(), "first");
}

#[test]
fn saves_second_report() {
    std::fs::write("report.txt", "second").unwrap();

    assert_eq!(std::fs::read_to_string("report.txt").unwrap(), "second");
}
```

Refactor until:

- Each test writes to a unique path
- Each test cleans up after itself
- The tests pass with normal parallel execution
- The tests still pass with `cargo test -- --test-threads=1`

Hint:

```rust
let mut path = std::env::temp_dir();
path.push(format!("study-report-{}", std::process::id()));
```

Add a suffix per test name so both tests do not use the same file.

---

## Chapter Checkpoint

You should now be able to answer:

- How do you run one test by name?
- Why does `-- --nocapture` have two dashes?
- What does `dbg!` print?
- When should you enable `RUST_BACKTRACE=1`?
- What does it mean to shrink a failure?
- Why do tests that share file paths become flaky?
- How can pure helper functions make environment-variable code easier to test?

---

## Recap

- Reproduce a failure locally before changing code.
- Run one test when the full suite is noisy.
- Use `-- --nocapture`, `dbg!`, and backtraces to inspect behavior.
- Shrink large failures into small examples.
- Fix flaky tests by removing nondeterminism and shared state.
- Trustworthy tests are deterministic tests.

## Try It Yourself

Find one test in your project that uses time, randomness, files, environment
variables, or network access. Refactor it so the core logic can be tested with
fixed input.

---

[**Next ->** Chapter 03 Capstone](./06-chapter-03-capstone.md)  
[**<- Previous** Rust CI Workflows with GitHub Actions](./04-rust-ci-workflows-with-github-actions.md)
