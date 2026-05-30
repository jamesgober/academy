<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 05](./README.md)

---

# Common Concurrency Mistakes

> Go makes it easy to start concurrent work. That is a gift and a trap. Most
> beginner concurrency bugs are coordination bugs, not syntax bugs.

**You will learn:**
- Why goroutine leaks happen
- Why deadlocks happen
- Why data races are dangerous
- Why loop variables can surprise beginners
- Why `WaitGroup`, channel closing, and context cancellation need clear ownership
- How to decide when not to use concurrency

**Before this page, you should know:** [Channels And Message Passing](./02-channels-and-message-passing.md)

---

## Mistake 1: Starting Goroutines With No Completion Plan

Bad:

```go
for _, name := range names {
    go process(name)
}
```

Problem:

```text
main may exit before goroutines finish.
errors have nowhere to go.
results have nowhere to go.
```

Better:

```go
var wg sync.WaitGroup

for _, name := range names {
    wg.Add(1)

    go func(name string) {
        defer wg.Done()
        process(name)
    }(name)
}

wg.Wait()
```

Every goroutine should have an answer to:

```text
Who waits for me?
How do I report failure?
How do I stop?
```

---

## Mistake 2: Deadlock

Deadlock means goroutines are waiting forever.

```go
func main() {
    ch := make(chan string)
    ch <- "hello"
}
```

The send waits for a receiver. There is no receiver.

Fix:

```go
func main() {
    ch := make(chan string)

    go func() {
        ch <- "hello"
    }()

    fmt.Println(<-ch)
}
```

Deadlock checklist:

```text
Is someone sending?
Is someone receiving?
Can both sides run?
Is a channel never closed?
Is a WaitGroup count never decremented?
```

---

## Mistake 3: Closing From The Wrong Place

Bad:

```go
func worker(jobs chan string) {
    defer close(jobs)
    // receives jobs...
}
```

Receivers usually should not close channels.

Rule:

```text
The goroutine that owns sending is usually responsible for closing.
```

Good:

```go
func produce(jobs chan<- string, names []string) {
    defer close(jobs)

    for _, name := range names {
        jobs <- name
    }
}
```

If multiple goroutines send to one channel, coordinate closing with a
`sync.WaitGroup` or a single closer goroutine.

---

## Mistake 4: Data Races

A data race happens when goroutines access the same variable at the same time,
at least one writes, and there is no synchronization.

Risky:

```go
count := 0

for i := 0; i < 100; i++ {
    go func() {
        count++
    }()
}
```

`count++` is not one indivisible operation. It reads, adds, and writes.

Better options:

- Send results through a channel and count in one goroutine
- Use `sync.Mutex`
- Use `sync/atomic` for simple counters

Race detector:

```bash
go test -race ./...
```

Use it. It catches real bugs.

---

## Mistake 5: Shared Memory When Messages Would Be Clearer

Messy:

```go
var reports []Report
var mu sync.Mutex

go func() {
    report := process("a.txt")
    mu.Lock()
    reports = append(reports, report)
    mu.Unlock()
}()
```

Often clearer:

```go
results := make(chan Report)

go func() {
    results <- process("a.txt")
}()

report := <-results
```

Mutexes are not bad. They are useful. But for beginner worker flows, channels
often make ownership clearer:

```text
worker owns work
worker sends result
coordinator owns final slice
```

---

## Mistake 6: Ignoring Cancellation

Long-running goroutines need a way to stop.

Common Go tool:

```go
context.Context
```

Example:

```go
func worker(ctx context.Context, jobs <-chan string) {
    for {
        select {
        case <-ctx.Done():
            return
        case job, ok := <-jobs:
            if !ok {
                return
            }
            process(job)
        }
    }
}
```

This worker stops when:

- Context is cancelled
- Jobs channel is closed

Without cancellation, goroutines can leak and keep waiting forever.

---

## Mistake 7: Assuming Concurrency Is Faster

Concurrency adds overhead:

- Goroutine scheduling
- Channel communication
- Locks
- More complex testing
- Harder debugging

Do not add goroutines to code that is already simple and fast.

Use concurrency when:

- Work can actually overlap
- I/O waits dominate
- CPU work can be split safely
- The coordination cost is worth it

Beginner rule:

```text
Make the single-threaded version clear first.
Add concurrency only when you can explain the work flow.
```

---

## Mistake 8: Loop Variable Confusion

Modern Go improved loop variable behavior, but you will still see older code and
older explanations.

Safe habit:

```go
for _, name := range names {
    name := name

    go func() {
        process(name)
    }()
}
```

Or pass the value:

```go
for _, name := range names {
    go func(name string) {
        process(name)
    }(name)
}
```

This makes it obvious each goroutine gets its own value.

---

## Concurrency Design Checklist

Before writing concurrent code, fill this out:

```text
Work units:
Producer:
Worker count:
Result type:
Error path:
Completion signal:
Cancellation signal:
Who closes each channel:
Shared state, if any:
How to test without timing guesses:
```

If any line is blank, pause.

---

## Mini Project: Find The Bug

Buggy code:

```go
func ProcessAll(names []string) []string {
    results := []string{}

    for _, name := range names {
        go func() {
            results = append(results, strings.ToUpper(name))
        }()
    }

    return results
}
```

Problems:

- Function returns before goroutines finish
- Multiple goroutines append to the same slice
- No error/completion plan
- Loop variable handling may confuse readers

Refactor plan:

```text
Use a results channel.
Use WaitGroup to know when workers finish.
Close results after workers are done.
Collect results in one goroutine.
```

---

## Chapter Checkpoint

You should now be able to answer:

- What is a goroutine leak?
- What causes a deadlock?
- Who should usually close a channel?
- What is a data race?
- How do you run Go's race detector?
- Why can channels be clearer than shared slices?
- What does `context.Context` help with?
- Why should concurrency come after a clear sequential design?

---

## Recap

- Every goroutine needs a completion and cancellation plan.
- Deadlocks are waiting problems.
- Data races are shared-memory problems.
- Channel closing belongs to the sending side.
- Use `go test -race ./...` to catch race bugs.
- Concurrency should clarify throughput, not hide design confusion.

## Try It Yourself

Take one concurrent example from this chapter and write the design checklist
before coding. Then run:

```bash
go test -race ./...
```

---

[**Next ->** A Small Worker Pattern](./04-a-small-worker-pattern.md)  
[**<- Previous** Channels And Message Passing](./02-channels-and-message-passing.md)
