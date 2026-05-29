<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 04](./README.md)

---

# Threads, Shared State, And Channels

> A thread is a separate path of execution. Rust makes you be honest about what
> each path owns, shares, locks, and sends.

**You will learn:**
- How to spawn and join threads
- Why `move` closures matter
- How `Arc<Mutex<T>>` shares mutable state safely
- How channels let threads send messages
- How to build a small worker example that feels like real code

**Before this page, you should know:** [Concurrency Safety In Rust](./01-concurrency-safety-in-rust.md)

---

## Your First Thread

```rust
use std::thread;

fn main() {
    let handle = thread::spawn(|| {
        println!("hello from the worker thread");
    });

    println!("hello from the main thread");

    handle.join().unwrap();
}
```

`thread::spawn` starts a new thread.

`join` waits for that thread to finish.

Without `join`, `main` might exit before the worker prints anything.

Visual model:

```text
main thread:
  spawn worker
  print main message
  wait for worker with join

worker thread:
  print worker message
  finish
```

---

## `JoinHandle`

`thread::spawn` returns a `JoinHandle`.

Think of it as a receipt for the thread you started.

```rust
let handle = thread::spawn(|| {
    42
});

let answer = handle.join().unwrap();
assert_eq!(answer, 42);
```

A thread closure can return a value. `join()` gives that value back.

`join()` returns a `Result` because a thread can panic.

```rust
let handle = thread::spawn(|| {
    panic!("worker failed");
});

let result = handle.join();

assert!(result.is_err());
```

In beginner examples, you will often see:

```rust
handle.join().unwrap();
```

That means:

> Wait for the thread, and panic if the thread panicked.

Production code may handle that error more carefully.

---

## Moving Data Into A Thread

Threads commonly need input.

```rust
use std::thread;

fn main() {
    let files = vec![
        String::from("notes.txt"),
        String::from("report.txt"),
        String::from("summary.txt"),
    ];

    let handle = thread::spawn(move || {
        for file in files {
            println!("would process {file}");
        }
    });

    handle.join().unwrap();
}
```

The `move` keyword moves `files` into the closure.

After that, `main` no longer owns `files`.

This is safe because the worker thread owns what it uses.

---

## Spawning Multiple Threads

```rust
use std::thread;

fn main() {
    let jobs = vec!["image-a.png", "image-b.png", "image-c.png"];
    let mut handles = Vec::new();

    for job in jobs {
        let handle = thread::spawn(move || {
            println!("processing {job}");
            job.len()
        });

        handles.push(handle);
    }

    for handle in handles {
        let length = handle.join().unwrap();
        println!("job name length: {length}");
    }
}
```

Each loop iteration creates a thread and moves that iteration's `job` into it.

Important:

```rust
for job in jobs {
```

This loop consumes the vector. Each `&str` item is copied into the thread
closure because string slices here point to static string literals.

With owned `String` values, each `String` would move into one thread.

---

## Shared Mutable State With `Arc<Mutex<T>>`

Sometimes threads need to update one shared value.

Example: completed job count.

You cannot safely do this with a plain `u32` shared everywhere. You need:

- Shared ownership
- Safe mutation

Rust standard library pieces:

```text
Arc<T>      lets multiple threads own the same value.
Mutex<T>    lets one thread at a time mutate the value.
```

Together:

```text
Arc<Mutex<T>>
```

Visual model:

```text
Thread A ----\
Thread B ----- Arc ---- Mutex ---- completed_count
Thread C ----/

Only one thread can hold the lock at a time.
```

Code:

```rust
use std::sync::{Arc, Mutex};
use std::thread;

fn main() {
    let completed = Arc::new(Mutex::new(0_u32));
    let mut handles = Vec::new();

    for _ in 0..4 {
        let completed = Arc::clone(&completed);

        let handle = thread::spawn(move || {
            let mut count = completed.lock().unwrap();
            *count += 1;
        });

        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    let final_count = completed.lock().unwrap();
    println!("completed jobs: {final_count}");
}
```

Line by line:

```rust
let completed = Arc::new(Mutex::new(0_u32));
```

Create one shared, locked counter.

```rust
let completed = Arc::clone(&completed);
```

Give another thread shared ownership of the same counter.

```rust
let mut count = completed.lock().unwrap();
```

Wait for the lock and get mutable access.

```rust
*count += 1;
```

Change the value inside the mutex guard.

When `count` goes out of scope, the lock is released.

---

## Keep Locks Short

Bad:

```rust
let mut count = completed.lock().unwrap();

slow_file_processing();

*count += 1;
```

This holds the lock while doing slow work. Other threads wait for no reason.

Better:

```rust
slow_file_processing();

let mut count = completed.lock().unwrap();
*count += 1;
```

Lock only while touching the shared data.

Beginner rule:

> Do work outside the lock. Update shared state inside the lock.

---

## Poisoned Mutexes

`lock()` returns a `Result`.

Why?

If a thread panics while holding a mutex, Rust marks the mutex as poisoned. That
means the protected data might be in an inconsistent state.

Beginner examples often use:

```rust
let mut value = mutex.lock().unwrap();
```

That is acceptable while learning. In production, you should decide what to do
when shared state may have been left half-updated.

---

## Channels

Channels let threads communicate by sending messages.

Mental model:

```text
Sender ---- message ----> Receiver
Sender ---- message ----> Receiver
Sender ---- message ----> Receiver
```

Rust's standard channel module is `std::sync::mpsc`.

`mpsc` means:

```text
multiple producer, single consumer
```

Many senders can send to one receiver.

---

## Basic Channel

```rust
use std::sync::mpsc;
use std::thread;

fn main() {
    let (tx, rx) = mpsc::channel();

    thread::spawn(move || {
        tx.send(String::from("done")).unwrap();
    });

    let message = rx.recv().unwrap();
    println!("received: {message}");
}
```

`tx` sends.

`rx` receives.

`recv()` blocks until a message arrives or all senders are dropped.

---

## Multiple Senders

Clone the sender:

```rust
use std::sync::mpsc;
use std::thread;

fn main() {
    let (tx, rx) = mpsc::channel();
    let jobs = vec!["alpha", "beta", "gamma"];

    for job in jobs {
        let tx = tx.clone();

        thread::spawn(move || {
            let message = format!("{job} complete");
            tx.send(message).unwrap();
        });
    }

    drop(tx);

    for message in rx {
        println!("{message}");
    }
}
```

Why `drop(tx)`?

The original sender still exists in `main`. If you do not drop it, the receiver
may keep waiting because at least one sender is still alive.

Visual model:

```text
tx original in main -- drop it when setup is done
tx clone in worker A -- dropped when worker A exits
tx clone in worker B -- dropped when worker B exits
tx clone in worker C -- dropped when worker C exits

receiver loop ends when all senders are gone
```

---

## Send Results, Not Just Strings

Real workers can fail.

```rust
use std::sync::mpsc;
use std::thread;

#[derive(Debug)]
struct JobResult {
    name: String,
    bytes_processed: usize,
}

fn process_job(name: &str) -> Result<JobResult, String> {
    if name.trim().is_empty() {
        return Err("job name cannot be empty".to_string());
    }

    Ok(JobResult {
        name: name.to_string(),
        bytes_processed: name.len() * 100,
    })
}

fn main() {
    let (tx, rx) = mpsc::channel();
    let jobs = vec!["alpha", "", "gamma"];

    for job in jobs {
        let tx = tx.clone();

        thread::spawn(move || {
            let result = process_job(job);
            tx.send(result).unwrap();
        });
    }

    drop(tx);

    for result in rx {
        match result {
            Ok(job) => println!("finished {}: {} bytes", job.name, job.bytes_processed),
            Err(error) => eprintln!("job failed: {error}"),
        }
    }
}
```

The main thread becomes a coordinator:

```text
Workers do work.
Workers send results.
Main thread reports outcomes.
```

---

## Shared State Versus Channels

Use shared state when:

- Many threads need to update the same small state
- The state naturally belongs in one place
- Locking is simple and short

Use channels when:

- Workers can send results instead of editing shared memory
- You want a coordinator to own final aggregation
- You want clearer ownership of messages

Comparison:

```text
Arc<Mutex<T>>
  Threads meet at a shared notebook.

Channel
  Threads send notes to a coordinator.
```

When in doubt, beginners should try channels first. Message flow is often easier
to reason about than shared mutable state.

---

## Mini Project: Worker Progress Reporter

Build a program that:

- Spawns one thread per job
- Processes each job name
- Sends a result back to main
- Counts completed jobs
- Prints failures separately

Starter:

```rust
use std::sync::mpsc;
use std::thread;

#[derive(Debug)]
struct JobReport {
    job_name: String,
    units_processed: usize,
}

fn process_job(job_name: &str) -> Result<JobReport, String> {
    if job_name.trim().is_empty() {
        return Err("job name cannot be empty".to_string());
    }

    Ok(JobReport {
        job_name: job_name.to_string(),
        units_processed: job_name.len() * 10,
    })
}

fn main() {
    let jobs = vec!["images", "logs", "", "exports"];
    let (tx, rx) = mpsc::channel();

    for job in jobs {
        let tx = tx.clone();

        thread::spawn(move || {
            let report = process_job(job);
            tx.send(report).unwrap();
        });
    }

    drop(tx);

    let mut completed = 0;
    let mut failed = 0;

    for report in rx {
        match report {
            Ok(report) => {
                completed += 1;
                println!(
                    "completed {} with {} units",
                    report.job_name, report.units_processed
                );
            }
            Err(error) => {
                failed += 1;
                eprintln!("failed: {error}");
            }
        }
    }

    println!("completed: {completed}, failed: {failed}");
}
```

Exercises:

- Add a job ID.
- Return a custom error enum instead of `String`.
- Sort completed reports before printing.
- Add tests for `process_job`.

---

## Chapter Checkpoint

You should now be able to answer:

- What does `thread::spawn` return?
- Why do you call `join()`?
- Why does a spawned closure often use `move`?
- What problem does `Arc<T>` solve?
- What problem does `Mutex<T>` solve?
- Why should locks be held for a short time?
- What does `mpsc` mean?
- Why might a receiver loop need `drop(tx)`?

---

## Recap

- Threads run work on separate execution paths.
- `JoinHandle` lets you wait for a thread and receive its return value.
- `move` transfers captured values into a thread closure.
- `Arc<Mutex<T>>` supports shared mutable state across threads.
- Channels support message passing between threads.
- Channels often make beginner concurrency easier to understand.

## Try It Yourself

Take the worker progress reporter and add tests for the pure `process_job`
function. Keep thread coordination in `main`, and keep business logic in testable
functions.

---

[**Next ->** Async And Await Mental Model](./03-async-and-await-mental-model.md)  
[**<- Previous** Concurrency Safety In Rust](./01-concurrency-safety-in-rust.md)
