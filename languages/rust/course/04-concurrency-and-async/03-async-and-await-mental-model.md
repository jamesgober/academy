<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 04](./README.md)

---

# Async And Await Mental Model

> Async Rust is not magic. It is Rust turning a function into a resumable task
> that an async runtime can poll.

**You will learn:**
- Why async exists
- What an `async fn` returns
- What `.await` actually means
- Why Rust needs an async runtime
- How async differs from threads
- How to read small Tokio examples without feeling lost

**Before this page, you should know:** [Threads, Shared State, And Channels](./02-threads-shared-state-and-channels.md)

---

## Why Async Exists

Some programs spend most of their time waiting:

- Waiting for a web response
- Waiting for a database
- Waiting for a file
- Waiting for a socket
- Waiting for a timer

If one thread blocks every time one operation waits, you may need many threads
to handle many users.

Async lets one or a few threads juggle many waiting tasks.

Visual model:

```text
Blocking style:

Thread 1: request A [work][wait........][work]
Thread 2: request B [work][wait........][work]
Thread 3: request C [work][wait........][work]

Async style:

Runtime thread:
  request A [work][await]
  request B       [work][await]
  request C             [work][await]
  request A                   [resume]
  request B                          [resume]
```

Async shines when the work waits often.

Async does not automatically make CPU-heavy code faster.

---

## What `async fn` Returns

This looks like it returns a `String`:

```rust
async fn load_username() -> String {
    "ada".to_string()
}
```

But calling it does not immediately run the body to completion.

```rust
let future = load_username();
```

The value is a future.

Plain language:

```text
A future is a value that represents work that may finish later.
```

More precise:

```text
An async function returns a state machine that can be polled.
```

Beginner mental model:

```text
Normal function:
  call -> runs now -> returns value

Async function:
  call -> creates future
  runtime polls future
  future may finish now or say "not ready yet"
```

---

## `.await`

`.await` means:

```text
Wait for this future to finish, but let the runtime run other async tasks while
this task is waiting.
```

Example:

```rust
async fn load_profile() -> String {
    let username = load_username().await;
    format!("profile for {username}")
}
```

When Rust reaches `.await`, the current async task may pause.

Visual model:

```text
load_profile task:

start
  |
  v
call load_username()
  |
  v
.await
  |
  |-- username ready now -> continue
  |
  `-- username pending  -> pause task
                         runtime does other work
                         resume later
```

`.await` is not the same as sleeping the whole thread. It pauses the task, not
necessarily the operating system thread.

---

## Async Needs A Runtime

The Rust standard library defines the `Future` trait, but it does not provide a
full production async runtime.

A runtime does the scheduling:

```text
Runtime:
  stores tasks
  polls futures
  wakes tasks when I/O is ready
  runs timers
  coordinates async work
```

Common Rust async runtimes include:

- Tokio
- async-std
- smol

Tokio is common in production Rust networking and server code.

Example `Cargo.toml` for Tokio:

```toml
[dependencies]
tokio = { version = "1", features = ["macros", "rt-multi-thread", "time"] }
```

Minimal Tokio program:

```rust
use std::time::Duration;
use tokio::time::sleep;

#[tokio::main]
async fn main() {
    println!("before wait");
    sleep(Duration::from_millis(100)).await;
    println!("after wait");
}
```

`#[tokio::main]` creates a runtime and runs your async `main` inside it.

Without a runtime, an async function creates a future that nobody drives.

---

## Async Is Lazy

This surprises beginners:

```rust
async fn say_hi() {
    println!("hi");
}

fn main() {
    let future = say_hi();
}
```

This creates a future, but it does not print `hi`.

The future must be awaited or spawned on a runtime:

```rust
#[tokio::main]
async fn main() {
    say_hi().await;
}
```

Think:

```text
Creating a future is like writing a task on a sticky note.
Awaiting it is like actually doing the task.
```

---

## Sequential Await Versus Concurrent Await

This code waits for one task, then the other:

```rust
use std::time::Duration;
use tokio::time::sleep;

async fn download(name: &str) -> String {
    sleep(Duration::from_millis(100)).await;
    format!("{name} done")
}

#[tokio::main]
async fn main() {
    let a = download("a").await;
    let b = download("b").await;

    println!("{a}, {b}");
}
```

Timeline:

```text
a waits 100ms
b waits 100ms
total about 200ms
```

Concurrent version:

```rust
use std::time::Duration;
use tokio::time::sleep;

async fn download(name: &str) -> String {
    sleep(Duration::from_millis(100)).await;
    format!("{name} done")
}

#[tokio::main]
async fn main() {
    let (a, b) = tokio::join!(download("a"), download("b"));

    println!("{a}, {b}");
}
```

Timeline:

```text
a waits 100ms
b waits 100ms at the same time
total about 100ms
```

`tokio::join!` runs futures concurrently on the same task.

It does not mean CPU parallelism. It means the waits can overlap.

---

## Spawning Async Tasks

You can ask the runtime to manage a task:

```rust
use std::time::Duration;
use tokio::time::sleep;

async fn do_work(id: u32) -> String {
    sleep(Duration::from_millis(50)).await;
    format!("job {id} done")
}

#[tokio::main]
async fn main() {
    let handle = tokio::spawn(async {
        do_work(1).await
    });

    let result = handle.await.unwrap();
    println!("{result}");
}
```

`tokio::spawn` returns a join handle.

This line:

```rust
let result = handle.await.unwrap();
```

means:

```text
await the task finishing
unwrap the task result
```

The task result can fail if the task panicked or was cancelled by aborting.

---

## Threads Versus Async

Use threads when:

- Work is CPU-heavy
- Blocking libraries must be used
- You need true parallelism across CPU cores
- The number of workers is modest and controlled

Use async when:

- Work is I/O-heavy
- There are many waiting tasks
- You are writing servers, clients, network tools, or high-concurrency services
- The libraries you use are async-aware

Do not block inside async tasks:

```rust
// Bad inside async code:
std::thread::sleep(std::time::Duration::from_secs(1));
```

Use async sleep:

```rust
tokio::time::sleep(std::time::Duration::from_secs(1)).await;
```

Blocking a runtime worker prevents other async tasks from making progress.

---

## Async Function Signatures

Async functions can return `Result`:

```rust
async fn fetch_user(id: u64) -> Result<String, String> {
    if id == 0 {
        return Err("id must be greater than zero".to_string());
    }

    Ok(format!("user-{id}"))
}
```

Use `?` inside async functions the same way you do in normal functions:

```rust
async fn fetch_profile(id: u64) -> Result<String, String> {
    let user = fetch_user(id).await?;

    Ok(format!("profile for {user}"))
}
```

The `?` returns early from the async function's future with `Err(...)`.

---

## Mini Project: Fake Concurrent Downloads

Create:

```bash
cargo new async-downloads
cd async-downloads
```

Add `Cargo.toml` dependency:

```toml
[dependencies]
tokio = { version = "1", features = ["macros", "rt-multi-thread", "time"] }
```

`src/main.rs`:

```rust
use std::time::Duration;
use tokio::time::sleep;

async fn fake_download(name: &str, delay_ms: u64) -> String {
    sleep(Duration::from_millis(delay_ms)).await;
    format!("{name} finished after {delay_ms}ms")
}

#[tokio::main]
async fn main() {
    let first = fake_download("profile", 300);
    let second = fake_download("settings", 100);
    let third = fake_download("notifications", 200);

    let (profile, settings, notifications) = tokio::join!(first, second, third);

    println!("{profile}");
    println!("{settings}");
    println!("{notifications}");
}
```

Run:

```bash
cargo run
```

What to notice:

- The slowest task takes 300ms.
- The total time should be close to 300ms, not 600ms.
- The tasks overlap because each one awaits a timer.

---

## Chapter Checkpoint

You should now be able to answer:

- Why does async help I/O-heavy programs?
- What does an `async fn` return when called?
- What does `.await` do?
- Why does Rust need an async runtime?
- What does `#[tokio::main]` provide?
- Why is `std::thread::sleep` bad inside async tasks?
- What is the difference between sequential awaits and `tokio::join!`?

---

## Recap

- Async functions create futures.
- Futures do not run to completion unless polled by a runtime.
- `.await` is a suspension point.
- Async is excellent for many waiting tasks.
- Threads are still better for CPU-heavy parallel work.
- Tokio is a common async runtime for real Rust applications.

## Try It Yourself

Modify the fake download project:

- Add a fourth task.
- Make one task return `Result<String, String>`.
- Use `?` in an async helper function.
- Print a final summary after all tasks finish.

---

[**Next ->** Async Error Handling, Timeouts, And Cancellation](./04-async-error-timeouts-and-cancellation.md)  
[**<- Previous** Threads, Shared State, And Channels](./02-threads-shared-state-and-channels.md)
