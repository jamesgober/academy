<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 04](./README.md)

---

# Concurrency Safety In Rust

> Concurrency means more than one piece of work is in progress. Rust's job is to
> help you do that without silently corrupting data.

**You will learn:**
- What concurrency and parallelism mean
- What a data race is
- Why Rust reuses ownership and borrowing rules for threads
- What `Send` and `Sync` mean in human language
- How to choose between threads, shared state, channels, and async

**Before this page, you should know:** [Chapter 03 Capstone](../03-testing-edge-cases-and-ci/06-chapter-03-capstone.md)

---

## Concurrency Versus Parallelism

People use these words loosely, but the difference helps.

Concurrency:

```text
Multiple jobs are in progress during the same time period.
```

Parallelism:

```text
Multiple jobs are literally running at the same instant.
```

Visual model:

```text
Concurrency on one worker:

time ->
Task A: [work] [wait]        [work]
Task B:        [work] [wait]       [work]

Parallelism on two workers:

time ->
Worker 1: Task A [work][work][work]
Worker 2: Task B [work][work][work]
```

Async Rust is usually about concurrency: while one task waits for I/O, another
task can make progress.

Threads can give parallelism: two CPU cores can run two threads at once.

---

## What Goes Wrong Without Safety

Imagine two threads editing the same counter:

```text
counter = 0

Thread A reads counter -> 0
Thread B reads counter -> 0
Thread A writes 1
Thread B writes 1

Expected counter = 2
Actual counter   = 1
```

Both threads did reasonable steps, but together they lost an update.

This is the beginner version of the danger. Real data races can be much worse:

- Half-written values
- Crashes
- Corrupted files
- Security bugs
- Problems that disappear when you add `println!`

That last one is especially painful. Timing bugs can hide when you try to look
at them.

---

## Data Race In Plain Language

A data race happens when all three are true:

```text
1. Two or more threads access the same data.
2. At least one thread writes to that data.
3. There is no synchronization controlling access.
```

Rust's safe code tries to make that combination impossible.

The same rule you learned earlier returns:

```text
Many readers are okay.
One writer is okay.
Readers and a writer at the same time are not okay.
Multiple writers at the same time are not okay.
```

Visual model:

```text
Allowed:

data
 |-- &T reader
 |-- &T reader
 `-- &T reader

Allowed:

data
 `-- &mut T writer

Rejected:

data
 |-- &T reader
 `-- &mut T writer

Rejected:

data
 |-- &mut T writer
 `-- &mut T writer
```

The key Rust idea:

> Thread safety is not a separate topic from ownership. It is ownership applied
> to more than one thread.

---

## Why Normal Borrowing Is Not Enough Across Threads

This does not compile:

```rust
use std::thread;

fn main() {
    let message = String::from("hello");

    thread::spawn(|| {
        println!("{message}");
    });
}
```

The spawned thread might outlive `main`'s local variable. Rust cannot allow the
new thread to borrow `message` without proof that the borrow stays valid.

Use `move`:

```rust
use std::thread;

fn main() {
    let message = String::from("hello");

    let handle = thread::spawn(move || {
        println!("{message}");
    });

    handle.join().unwrap();
}
```

`move` tells the closure to take ownership of captured values.

After `message` moves into the thread, `main` cannot use it:

```rust
use std::thread;

fn main() {
    let message = String::from("hello");

    let handle = thread::spawn(move || {
        println!("{message}");
    });

    // println!("{message}"); // error: value was moved

    handle.join().unwrap();
}
```

Rust prevents the classic mistake:

```text
Main thread frees value.
Worker thread still tries to use value.
Program explodes later.
```

---

## `Send`

`Send` means:

```text
This value can be moved to another thread safely.
```

Many normal Rust types are `Send`:

- `String`
- `Vec<T>` when `T` is `Send`
- `u32`
- `bool`
- Owned structs made of `Send` fields

Example:

```rust
use std::thread;

#[derive(Debug)]
struct Job {
    id: u32,
    description: String,
}

fn main() {
    let job = Job {
        id: 1,
        description: String::from("resize image"),
    };

    let handle = thread::spawn(move || {
        println!("working on job {job:?}");
    });

    handle.join().unwrap();
}
```

`Job` can move into the thread because its fields can move into the thread.

---

## `Sync`

`Sync` means:

```text
References to this value can be shared between threads safely.
```

A type `T` is `Sync` when `&T` can be sent to another thread safely.

Beginner translation:

```text
Send: can I move the value to another thread?
Sync: can multiple threads safely hold shared references to it?
```

You usually do not implement `Send` or `Sync` yourself. The compiler and the
standard library compose them for normal types.

---

## Why `Rc<T>` Is Not Thread-Safe

`Rc<T>` means reference-counted pointer for single-threaded code.

It is not `Send` or `Sync`.

Why?

The reference count inside `Rc<T>` is not updated with thread-safe operations.
If two threads clone or drop the same `Rc<T>` at the same time, the count can be
corrupted.

Use `Arc<T>` for shared ownership across threads.

```text
Rc<T>  -> reference counting for one thread
Arc<T> -> atomic reference counting across threads
```

You will use `Arc<T>` in the next lesson.

---

## Four Concurrency Tools

Rust gives you several patterns. The beginner mistake is using one pattern for
everything.

| Tool | Best for | Beginner mental model |
|---|---|---|
| Thread | CPU work or blocking work in parallel | Hire another worker |
| Mutex | Shared mutable state | Put a lock on a notebook |
| Channel | Message passing between workers | Send notes through a mailbox |
| Async | Many I/O tasks waiting often | One worker juggling waiting tasks |

CPU-bound task:

```text
Compress images
Hash files
Parse many large documents
Do math-heavy work
```

Threads often fit.

I/O-bound task:

```text
Download many URLs
Wait for database queries
Handle many sockets
Read many slow files
```

Async often fits.

Shared state:

```text
Workers update one shared progress counter.
```

`Arc<Mutex<T>>` can fit.

Message passing:

```text
Workers send completed results back to coordinator.
```

Channels can fit.

---

## What Rust Prevents And What It Does Not

Rust prevents many memory and data-race bugs in safe code.

Rust does not automatically prevent:

- Deadlocks
- Slow code
- Bad task design
- Starvation
- Holding a lock for too long
- Incorrect business logic
- Race conditions around external systems

Example deadlock idea:

```text
Thread A locks Account 1, then waits for Account 2.
Thread B locks Account 2, then waits for Account 1.
Both wait forever.
```

Rust can guarantee memory safety here. It cannot magically know the correct
order to lock your bank accounts.

That is why this chapter teaches design, not just syntax.

---

## Mini Project: Classify Work

For each job, choose the likely Rust tool:

```text
1. Resize 500 images on a laptop.
2. Download 500 small web pages.
3. Let four workers increment one shared completed-job count.
4. Send parsed records from worker threads to a main summarizer.
5. Read from 10,000 network sockets.
```

Suggested answers:

```text
1. Threads or a thread pool.
2. Async.
3. Arc<Mutex<u32>> or an atomic counter.
4. Channel.
5. Async.
```

There can be more than one reasonable answer. The important part is explaining
the tradeoff.

---

## Chapter Checkpoint

You should now be able to answer:

- What is the difference between concurrency and parallelism?
- What three conditions create a data race?
- Why does `thread::spawn` often use `move`?
- What does `Send` mean?
- What does `Sync` mean?
- Why is `Rc<T>` not used across threads?
- When might you choose channels instead of shared state?
- What bugs can Rust still not design away for you?

---

## Recap

- Concurrency means multiple jobs are in progress.
- Parallelism means multiple jobs run at the same instant.
- Rust applies ownership and borrowing rules across threads.
- `Send` means a value can move to another thread.
- `Sync` means shared references can be used across threads.
- Threads, mutexes, channels, and async solve different problems.

## Try It Yourself

Write a short design note for a program that scans a folder of files and reports
word counts. Decide:

- What runs in worker threads?
- What data is shared?
- What messages are sent?
- What result comes back to the main thread?

---

[**Next ->** Threads, Shared State, And Channels](./02-threads-shared-state-and-channels.md)  
[**<- Previous** Chapter 04: Concurrency And Async](./README.md)
