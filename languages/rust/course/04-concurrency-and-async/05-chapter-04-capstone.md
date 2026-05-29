<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 04](./README.md)

---

# Chapter 04 Capstone: Threaded Job Dispatcher

> Build a small dispatcher that sends jobs to worker threads, receives results
> through a channel, tracks failures, and documents how an async version would
> handle timeouts and cancellation.

This capstone is intentionally practical. It looks like the core of many real
tools:

- Process several jobs
- Run work concurrently
- Collect results
- Report errors
- Keep shared state minimal
- Make the design explainable

---

## What You Are Building

You will build `job-dispatcher`.

The program will:

1. Accept a list of job names.
2. Spawn one worker thread per job.
3. Process each job with a pure function.
4. Send each result back to the main thread through a channel.
5. Count successes and failures.
6. Print a final report.
7. Include tests for the pure logic.
8. Include a design note for async timeout and cancellation behavior.

---

## Target Project Shape

```text
job-dispatcher/
  Cargo.toml
  src/
    lib.rs
    main.rs
  docs/
    async-policy.md
```

Create it:

```bash
cargo new job-dispatcher
cd job-dispatcher
```

---

## Step 1: Move Logic Into A Library

Beginner Rust projects often put everything in `main.rs`. That makes testing
hard.

Use:

```text
src/lib.rs   -> testable logic
src/main.rs  -> command-line wiring and printing
```

`src/lib.rs`:

```rust
#[derive(Debug, Clone, PartialEq, Eq)]
pub struct Job {
    name: String,
    units: usize,
}

#[derive(Debug, Clone, PartialEq, Eq)]
pub struct JobReport {
    job_name: String,
    units_processed: usize,
}

#[derive(Debug, Clone, PartialEq, Eq)]
pub enum JobError {
    EmptyName,
    NoUnits { job_name: String },
}
```

Add constructors and accessors:

```rust
impl Job {
    pub fn new(name: impl Into<String>, units: usize) -> Result<Self, JobError> {
        let name = name.into();

        if name.trim().is_empty() {
            return Err(JobError::EmptyName);
        }

        if units == 0 {
            return Err(JobError::NoUnits { job_name: name });
        }

        Ok(Self { name, units })
    }

    pub fn name(&self) -> &str {
        &self.name
    }

    pub fn units(&self) -> usize {
        self.units
    }
}

impl JobReport {
    pub fn job_name(&self) -> &str {
        &self.job_name
    }

    pub fn units_processed(&self) -> usize {
        self.units_processed
    }
}
```

---

## Step 2: Process One Job

Add:

```rust
pub fn process_job(job: Job) -> Result<JobReport, JobError> {
    Ok(JobReport {
        units_processed: job.units(),
        job_name: job.name,
    })
}
```

This is deliberately pure:

- No threads
- No channels
- No files
- No global state

That makes it easy to test.

---

## Step 3: Define Dispatcher Output

Add:

```rust
#[derive(Debug, Clone, PartialEq, Eq)]
pub struct DispatchSummary {
    reports: Vec<JobReport>,
    errors: Vec<JobError>,
}

impl DispatchSummary {
    pub fn reports(&self) -> &[JobReport] {
        &self.reports
    }

    pub fn errors(&self) -> &[JobError] {
        &self.errors
    }

    pub fn success_count(&self) -> usize {
        self.reports.len()
    }

    pub fn failure_count(&self) -> usize {
        self.errors.len()
    }
}
```

The summary owns the final results. Callers can inspect slices.

---

## Step 4: Dispatch Jobs With Threads And A Channel

Add:

```rust
use std::sync::mpsc;
use std::thread;

pub fn dispatch_jobs(jobs: Vec<Result<Job, JobError>>) -> DispatchSummary {
    let (tx, rx) = mpsc::channel();
    let mut handles = Vec::new();

    for job in jobs {
        let tx = tx.clone();

        let handle = thread::spawn(move || {
            let result = match job {
                Ok(job) => process_job(job),
                Err(error) => Err(error),
            };

            tx.send(result).expect("receiver should still be alive");
        });

        handles.push(handle);
    }

    drop(tx);

    let mut reports = Vec::new();
    let mut errors = Vec::new();

    for result in rx {
        match result {
            Ok(report) => reports.push(report),
            Err(error) => errors.push(error),
        }
    }

    for handle in handles {
        handle.join().expect("worker thread should not panic");
    }

    reports.sort_by(|left, right| left.job_name.cmp(&right.job_name));

    DispatchSummary { reports, errors }
}
```

Why accept `Vec<Result<Job, JobError>>`?

Because job creation can fail before dispatch. This lets the dispatcher report
both creation errors and processing errors in one summary.

Why sort reports?

Thread completion order is nondeterministic. Sorting gives stable output and
stable tests.

---

## Step 5: Add Tests

At the bottom of `src/lib.rs`:

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn creates_valid_job() {
        let job = Job::new("images", 10).unwrap();

        assert_eq!(job.name(), "images");
        assert_eq!(job.units(), 10);
    }

    #[test]
    fn rejects_empty_job_name() {
        let error = Job::new(" ", 10).unwrap_err();

        assert_eq!(error, JobError::EmptyName);
    }

    #[test]
    fn rejects_zero_units() {
        let error = Job::new("images", 0).unwrap_err();

        assert_eq!(
            error,
            JobError::NoUnits {
                job_name: "images".to_string()
            }
        );
    }

    #[test]
    fn processes_job() {
        let job = Job::new("images", 10).unwrap();
        let report = process_job(job).unwrap();

        assert_eq!(report.job_name(), "images");
        assert_eq!(report.units_processed(), 10);
    }

    #[test]
    fn dispatches_successes_and_failures() {
        let jobs = vec![
            Job::new("images", 10),
            Job::new("", 5),
            Job::new("logs", 3),
            Job::new("empty", 0),
        ];

        let summary = dispatch_jobs(jobs);

        assert_eq!(summary.success_count(), 2);
        assert_eq!(summary.failure_count(), 2);

        let names: Vec<_> = summary
            .reports()
            .iter()
            .map(JobReport::job_name)
            .collect();

        assert_eq!(names, vec!["images", "logs"]);
    }
}
```

The dispatcher uses threads, but the tests are still deterministic because
output is sorted.

---

## Step 6: Wire `main.rs`

`src/main.rs`:

```rust
use job_dispatcher::{dispatch_jobs, Job};

fn main() {
    let jobs = vec![
        Job::new("images", 10),
        Job::new("logs", 3),
        Job::new("", 5),
        Job::new("exports", 7),
    ];

    let summary = dispatch_jobs(jobs);

    println!("Successes: {}", summary.success_count());
    for report in summary.reports() {
        println!(
            "- {} processed {} units",
            report.job_name(),
            report.units_processed()
        );
    }

    println!("Failures: {}", summary.failure_count());
    for error in summary.errors() {
        println!("- {error:?}");
    }
}
```

Run:

```bash
cargo run
```

Then:

```bash
cargo test
```

---

## Step 7: Write The Async Policy

Create `docs/async-policy.md`:

```markdown
# Async Policy

## Future Async Version

The current dispatcher uses threads because each job represents CPU-style work.
If jobs later become I/O-heavy, such as downloading URLs or calling APIs, the
dispatcher may gain an async implementation.

## Timeout Rules

- User-facing jobs should have short timeouts.
- Background jobs may have longer timeouts.
- Every external operation must have a timeout.
- Timeout errors must include the job name and timeout duration.

## Cancellation Rules

- Cancellation is normal during shutdown.
- A cancelled job must not leave partial output marked as complete.
- If a job writes output, it should write to a temporary path first and rename
  only after success.
- Spawned async tasks must be awaited or explicitly aborted.

## Retry Rules

- Retry only temporary errors.
- Do not retry validation errors.
- Do not retry non-idempotent operations unless an idempotency key exists.
- Limit retry attempts.
- Log every failed attempt with job context.
```

This design note matters. It proves you understand async reliability even before
you rewrite the dispatcher with Tokio.

---

## Validation Checklist

Run:

```bash
cargo fmt --all
cargo clippy --all-targets --all-features -- -D warnings
cargo test
```

Expected:

- Formatting is clean.
- Clippy has no warnings.
- Tests pass repeatedly.
- Output order is stable.
- No shared mutable state is used unnecessarily.
- Worker threads are joined.
- Channel senders are dropped so the receiver loop can finish.

---

## Stretch Goals

After the required version works:

- Add job durations with `std::time::Instant`.
- Add a maximum worker count instead of one thread per job.
- Add a CLI argument parser.
- Add integration tests for the public library API.
- Add an async version using Tokio tasks.
- Add timeout tests for the async version.

Do not jump to these until the base version is clean.

---

## Reviewer Checklist

- Is the library/application split clear?
- Is pure logic tested without threads?
- Is threaded coordination small and readable?
- Are errors represented as types, not only strings?
- Are thread joins handled?
- Is result order deterministic?
- Is shared mutable state avoided where channels work better?
- Does the async policy mention timeouts, cancellation, retries, and partial state?

---

## What You Should Understand Now

You have now practiced:

- Spawning worker threads
- Moving data into closures
- Returning data through channels
- Joining threads
- Avoiding shared mutable state
- Sorting nondeterministic results
- Testing pure logic
- Documenting async reliability rules

That is the heart of useful Rust concurrency: not just making things run at the
same time, but making the result understandable.

---

## Next

Continue to [Chapter 05: Crates, Modules, Libraries, And Project Design](../05-crates-modules-libraries-and-project-design/README.md).

---

[**Next ->** Chapter 05: Crates, Modules, Libraries, And Project Design](../05-crates-modules-libraries-and-project-design/README.md)  
[**<- Previous** Async Error Handling, Timeouts, And Cancellation](./04-async-error-timeouts-and-cancellation.md)
