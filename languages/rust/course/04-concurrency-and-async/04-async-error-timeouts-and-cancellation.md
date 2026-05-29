<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 04](./README.md)

---

# Async Error Handling, Timeouts, And Cancellation

> Async code fails in all the normal ways, plus a few timing-shaped ways. A good
> async program decides what happens when work is slow, cancelled, retried, or
> only half complete.

**You will learn:**
- How `Result` works in async functions
- Why every external operation should have a timeout policy
- How `tokio::time::timeout` works
- What cancellation means in Rust async
- How to design retries without making things worse
- How to keep cleanup and partial updates understandable

**Before this page, you should know:** [Async And Await Mental Model](./03-async-and-await-mental-model.md)

---

## Async Errors Are Still `Result`

Async does not replace `Result`.

```rust
async fn load_user(id: u64) -> Result<String, String> {
    if id == 0 {
        return Err("user id must be greater than zero".to_string());
    }

    Ok(format!("user-{id}"))
}
```

Use `?` normally:

```rust
async fn load_profile(id: u64) -> Result<String, String> {
    let user = load_user(id).await?;

    Ok(format!("profile for {user}"))
}
```

The order matters:

```rust
load_user(id).await?
```

Means:

```text
1. Wait for the future to finish.
2. If it returned Err, return that Err from this function.
3. If it returned Ok(value), unwrap value and continue.
```

---

## Add Context To Errors

This error is weak:

```text
request failed
```

This error is better:

```text
loading profile for user 42 failed: request timed out after 2s
```

Context helps the person debugging.

Example:

```rust
async fn load_user(id: u64) -> Result<String, String> {
    if id == 0 {
        return Err("user id must be greater than zero".to_string());
    }

    Ok(format!("user-{id}"))
}

async fn load_profile(id: u64) -> Result<String, String> {
    let user = load_user(id)
        .await
        .map_err(|error| format!("loading user {id} failed: {error}"))?;

    Ok(format!("profile for {user}"))
}
```

This pattern is common until you introduce a richer error crate or custom error
type.

---

## Timeouts

A timeout is a rule:

```text
If this operation does not finish within this duration, stop waiting and return
a timeout error.
```

Why timeouts matter:

- A server can hang forever.
- A database query can stall.
- A network can disappear.
- A user may not want to wait.
- A background worker may need to move on.

Tokio example:

```rust
use std::time::Duration;
use tokio::time::{sleep, timeout};

async fn slow_operation() -> &'static str {
    sleep(Duration::from_secs(5)).await;
    "done"
}

#[tokio::main]
async fn main() {
    let result = timeout(Duration::from_secs(1), slow_operation()).await;

    match result {
        Ok(value) => println!("operation finished: {value}"),
        Err(_) => eprintln!("operation timed out"),
    }
}
```

The result type is nested:

```text
timeout(...).await
  -> Result<operation_output, timeout_error>
```

If the operation itself returns `Result`, you get:

```text
Result<Result<T, OperationError>, TimeoutError>
```

Handle both layers.

---

## Timeout Around A Fallible Operation

```rust
use std::time::Duration;
use tokio::time::{sleep, timeout};

async fn fetch_settings(user_id: u64) -> Result<String, String> {
    if user_id == 0 {
        return Err("invalid user id".to_string());
    }

    sleep(Duration::from_millis(100)).await;
    Ok("dark-mode=true".to_string())
}

async fn fetch_settings_with_timeout(user_id: u64) -> Result<String, String> {
    let result = timeout(Duration::from_secs(1), fetch_settings(user_id))
        .await
        .map_err(|_| format!("fetching settings for user {user_id} timed out"))?;

    result.map_err(|error| format!("fetching settings for user {user_id} failed: {error}"))
}
```

Read it slowly:

```rust
.await
```

Wait for either the operation or the timeout.

```rust
.map_err(|_| "... timed out")?
```

If timeout happened, return a timeout error.

```rust
result.map_err(...)
```

If the operation finished but returned its own error, add context.

---

## Choosing Timeout Durations

There is no universal timeout.

Ask:

```text
Is a human waiting?
Is this a background job?
Can the operation be retried safely?
How expensive is this operation?
What is the normal response time?
What happens if we give up too early?
What happens if we wait too long?
```

Examples:

| Operation | Possible timeout |
|---|---|
| UI autocomplete request | 200ms to 1s |
| User-facing API request | 1s to 5s |
| Background sync | 30s to several minutes |
| Large upload | Based on file size and progress |
| Internal health check | Short and strict |

These are not laws. They are starting points for design.

---

## Cancellation

Cancellation means:

```text
Work started, but someone decided it should stop before normal completion.
```

Reasons:

- User navigated away
- Request timed out
- Parent task failed
- Program is shutting down
- A faster competing task already succeeded

In Rust async, dropping a future usually cancels it.

Example:

```rust
use std::time::Duration;
use tokio::time::{sleep, timeout};

async fn long_task() {
    println!("task started");
    sleep(Duration::from_secs(10)).await;
    println!("task finished");
}

#[tokio::main]
async fn main() {
    let result = timeout(Duration::from_millis(100), long_task()).await;

    if result.is_err() {
        println!("task timed out and was cancelled");
    }
}
```

After the timeout, `long_task` does not keep running in this direct form.

Cancellation is normal. Design for it.

---

## Spawned Task Cancellation

When you spawn a task, it can keep running independently.

```rust
use std::time::Duration;
use tokio::time::sleep;

#[tokio::main]
async fn main() {
    let handle = tokio::spawn(async {
        sleep(Duration::from_secs(10)).await;
        println!("finished");
    });

    handle.abort();

    match handle.await {
        Ok(_) => println!("task completed"),
        Err(error) if error.is_cancelled() => println!("task was cancelled"),
        Err(error) => println!("task failed: {error}"),
    }
}
```

`abort()` asks Tokio to cancel the task.

You still await the handle to observe the cancellation result.

---

## Cleanup And Partial State

Cancellation gets dangerous when a task changes state in multiple steps.

Risky flow:

```text
1. Mark invoice as paid.
2. Send receipt email.
3. Write audit log.
```

If cancellation happens after step 1, what is true?

```text
Invoice says paid.
Receipt may not be sent.
Audit log may be missing.
```

Design options:

- Make the operation transactional
- Save progress after each step
- Make steps retryable
- Use a background queue
- Make cleanup explicit
- Keep critical sections small

Rust keeps memory safe. You still design business correctness.

---

## Retries

A retry means trying again after a failure.

Retries help when failures are temporary:

- Network blip
- Rate limit
- Temporary service outage
- Lock contention

Retries are bad when the operation is not safe to repeat.

Danger:

```text
Charge credit card.
Timeout before response arrives.
Retry.
Customer is charged twice.
```

Retry design questions:

```text
Is this operation idempotent?
How many attempts?
How long between attempts?
Which errors are retryable?
When do we stop?
How do we log attempts?
```

Idempotent means:

```text
Doing the same operation multiple times has the same effect as doing it once.
```

Example retry loop:

```rust
use std::time::Duration;
use tokio::time::sleep;

async fn unreliable_fetch(attempt: u32) -> Result<String, String> {
    if attempt < 3 {
        Err(format!("temporary failure on attempt {attempt}"))
    } else {
        Ok("data".to_string())
    }
}

async fn fetch_with_retries(max_attempts: u32) -> Result<String, String> {
    for attempt in 1..=max_attempts {
        match unreliable_fetch(attempt).await {
            Ok(value) => return Ok(value),
            Err(error) if attempt < max_attempts => {
                eprintln!("{error}; retrying");
                sleep(Duration::from_millis(100)).await;
            }
            Err(error) => return Err(error),
        }
    }

    unreachable!("loop always returns");
}
```

This is simple. Real systems often use exponential backoff and jitter, but the
core idea is the same.

---

## Mini Project: Timeout-Wrapped Fetch

Create a fake async operation:

```rust
use std::time::Duration;
use tokio::time::{sleep, timeout};

#[derive(Debug, PartialEq, Eq)]
enum FetchError {
    InvalidId,
    Timeout,
    Failed(String),
}

async fn fetch_report(id: u64, delay: Duration) -> Result<String, FetchError> {
    if id == 0 {
        return Err(FetchError::InvalidId);
    }

    sleep(delay).await;
    Ok(format!("report-{id}"))
}

async fn fetch_report_with_timeout(
    id: u64,
    delay: Duration,
    limit: Duration,
) -> Result<String, FetchError> {
    timeout(limit, fetch_report(id, delay))
        .await
        .map_err(|_| FetchError::Timeout)?
}
```

Add tests with Tokio:

```toml
[dev-dependencies]
tokio = { version = "1", features = ["macros", "rt-multi-thread", "time"] }
```

```rust
#[tokio::test]
async fn rejects_invalid_id() {
    let result = fetch_report_with_timeout(
        0,
        Duration::from_millis(1),
        Duration::from_millis(50),
    )
    .await;

    assert_eq!(result, Err(FetchError::InvalidId));
}

#[tokio::test]
async fn times_out_when_operation_is_too_slow() {
    let result = fetch_report_with_timeout(
        42,
        Duration::from_millis(100),
        Duration::from_millis(1),
    )
    .await;

    assert_eq!(result, Err(FetchError::Timeout));
}
```

The tests prove two different failure modes:

- The operation rejected bad input.
- The timeout wrapper stopped waiting.

---

## Chapter Checkpoint

You should now be able to answer:

- How does `?` work after `.await`?
- Why should async errors include context?
- What does `tokio::time::timeout` return?
- Why can nested `Result` happen with timeouts?
- What does cancellation mean?
- Why can partial state be dangerous?
- What does idempotent mean?
- Why are retries risky for payments or other side effects?

---

## Recap

- Async functions still use `Result`.
- Timeouts prevent endless waits.
- Cancellation is normal and should be designed.
- Spawned tasks can be aborted and awaited.
- Retries need limits and idempotency.
- Rust gives safety tools, but you still design correct workflows.

## Try It Yourself

Extend the timeout-wrapped fetch project:

- Add a retry wrapper.
- Retry only `FetchError::Failed`.
- Do not retry `InvalidId`.
- Stop after three attempts.
- Write async tests for success, timeout, and non-retryable errors.

---

[**Next ->** Chapter 04 Capstone](./05-chapter-04-capstone.md)  
[**<- Previous** Async And Await Mental Model](./03-async-and-await-mental-model.md)
