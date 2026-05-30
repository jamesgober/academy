<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 05](./README.md)

---

# A Small Worker Pattern

> A worker pattern gives repeated jobs to goroutines in a controlled way. It is
> the difference between "random goroutines everywhere" and a system you can
> explain.

**You will learn:**
- What a worker pattern solves
- How jobs, workers, results, and errors flow
- How `sync.WaitGroup` and channels cooperate
- How to close channels safely
- How to build a small worker pool without hand-waving
- How to test the pure work separately from concurrent coordination

**Before this page, you should know:** [Common Concurrency Mistakes](./03-common-concurrency-mistakes.md)

---

## The Flow

```text
jobs channel
  |
  |-- worker 1 --\
  |-- worker 2 ----> results channel --> coordinator
  |-- worker 3 --/
```

Plain language:

```text
One goroutine sends jobs.
Several worker goroutines receive jobs.
Each worker sends a result.
One coordinator collects results.
```

The worker pattern gives concurrency a shape.

---

## What You Are Building

You will process study files.

Each job is a filename.

Each result says:

- Which file was processed
- How many "units" were processed
- Whether an error happened

This is fake file processing so the concurrency stays visible. In a real app,
the worker might read files, call APIs, resize images, or parse logs.

---

## Define The Types

```go
package main

import (
    "fmt"
    "strings"
    "sync"
)

type Job struct {
    ID   int
    Name string
}

type Result struct {
    JobID int
    Name  string
    Units int
    Err   error
}
```

Why include `Err` in `Result`?

Because workers should not just disappear when work fails. They should report
the failure.

---

## Write Pure Work First

```go
func process(job Job) Result {
    name := strings.TrimSpace(job.Name)
    if name == "" {
        return Result{
            JobID: job.ID,
            Name:  job.Name,
            Err:   fmt.Errorf("job name cannot be empty"),
        }
    }

    return Result{
        JobID: job.ID,
        Name:  name,
        Units: len(name) * 10,
    }
}
```

This function has no goroutines and no channels.

That is good.

Test the pure work before testing coordination:

```go
func TestProcessRejectsEmptyName(t *testing.T) {
    result := process(Job{ID: 1, Name: " "})

    if result.Err == nil {
        t.Fatal("expected error")
    }
}
```

---

## Worker Function

```go
func worker(id int, jobs <-chan Job, results chan<- Result, wg *sync.WaitGroup) {
    defer wg.Done()

    for job := range jobs {
        result := process(job)
        results <- result
    }
}
```

Read the signature:

```go
id int
```

Worker identifier for logging/debugging.

```go
jobs <-chan Job
```

Receive-only channel. Worker receives jobs.

```go
results chan<- Result
```

Send-only channel. Worker sends results.

```go
wg *sync.WaitGroup
```

Shared completion tracker.

The worker exits when `jobs` is closed and drained.

---

## Dispatch Function

```go
func Dispatch(jobsList []Job, workerCount int) []Result {
    jobs := make(chan Job)
    results := make(chan Result)

    var wg sync.WaitGroup

    for id := 1; id <= workerCount; id++ {
        wg.Add(1)
        go worker(id, jobs, results, &wg)
    }

    go func() {
        for _, job := range jobsList {
            jobs <- job
        }
        close(jobs)
    }()

    go func() {
        wg.Wait()
        close(results)
    }()

    collected := make([]Result, 0, len(jobsList))
    for result := range results {
        collected = append(collected, result)
    }

    return collected
}
```

Key ownership rules:

```text
Producer goroutine closes jobs.
Closer goroutine closes results after workers finish.
Coordinator ranges over results.
```

No worker closes `results`, because multiple workers send there.

---

## Why The Results Closer Is Separate

This part matters:

```go
go func() {
    wg.Wait()
    close(results)
}()
```

Workers send to `results`.

The coordinator ranges over `results`.

The range ends only when `results` is closed.

But `results` should close only after all workers are done sending.

That is exactly what the closer goroutine does.

Visual model:

```text
workers finish
     |
     v
wg.Wait returns
     |
     v
close(results)
     |
     v
range results ends
```

---

## Main Program

```go
func main() {
    jobs := []Job{
        {ID: 1, Name: "alpha"},
        {ID: 2, Name: " "},
        {ID: 3, Name: "gamma"},
        {ID: 4, Name: "delta"},
    }

    results := Dispatch(jobs, 2)

    for _, result := range results {
        if result.Err != nil {
            fmt.Printf("job %d failed: %v\n", result.JobID, result.Err)
            continue
        }

        fmt.Printf("job %d processed %s: %d units\n", result.JobID, result.Name, result.Units)
    }
}
```

Output order may vary because workers finish in different orders.

If stable order matters, sort results before printing.

---

## Add Stable Ordering

```go
sort.Slice(results, func(i, j int) bool {
    return results[i].JobID < results[j].JobID
})
```

Use stable ordering in tests so concurrent timing does not make tests flaky.

---

## Test The Dispatcher

```go
func TestDispatchReturnsOneResultPerJob(t *testing.T) {
    jobs := []Job{
        {ID: 1, Name: "alpha"},
        {ID: 2, Name: "beta"},
        {ID: 3, Name: ""},
    }

    results := Dispatch(jobs, 2)

    if len(results) != len(jobs) {
        t.Fatalf("got %d results, want %d", len(results), len(jobs))
    }
}
```

Better test with sorting:

```go
func TestDispatchReportsErrors(t *testing.T) {
    jobs := []Job{
        {ID: 1, Name: "alpha"},
        {ID: 2, Name: ""},
    }

    results := Dispatch(jobs, 2)
    sort.Slice(results, func(i, j int) bool {
        return results[i].JobID < results[j].JobID
    })

    if results[0].Err != nil {
        t.Fatalf("job 1 error = %v", results[0].Err)
    }
    if results[1].Err == nil {
        t.Fatal("job 2 expected error")
    }
}
```

---

## Worker Count Decisions

Do not always use one worker per job.

Ask:

```text
Is work CPU-heavy?
Is work I/O-heavy?
How many jobs exist?
Does external service rate-limit us?
How much memory does each job use?
```

Examples:

| Situation | Worker count idea |
|---|---|
| Small learning project | 2 or 3 |
| CPU-heavy work | Around CPU core count |
| I/O-heavy work | More workers may help |
| External API | Respect rate limits |
| Huge memory per job | Fewer workers |

Concurrency is a throttle, not just a speed button.

---

## Add Cancellation Later

A production worker often accepts `context.Context`:

```go
func worker(ctx context.Context, jobs <-chan Job, results chan<- Result, wg *sync.WaitGroup) {
    defer wg.Done()

    for {
        select {
        case <-ctx.Done():
            return
        case job, ok := <-jobs:
            if !ok {
                return
            }
            results <- process(job)
        }
    }
}
```

Learn the basic worker pattern first. Add context when you need cancellation,
timeouts, or request lifetimes.

---

## Chapter Checkpoint

You should now be able to answer:

- What are the four parts of a worker pattern?
- Why does the producer close `jobs`?
- Why do workers not close `results`?
- Why does a closer goroutine wait for the `WaitGroup`?
- Why can result order vary?
- Why should pure work be tested separately?
- How do directional channel parameters help?
- When should worker count be limited?

---

## Recap

- Worker patterns give repeated concurrent work a clear shape.
- Jobs flow into workers; results flow back to a coordinator.
- `WaitGroup` tracks worker completion.
- Channel closing must have clear ownership.
- Sort concurrent results when stable output matters.
- Keep business logic pure and testable.

## Try It Yourself

Extend the dispatcher:

- Add `StartedAt` and `FinishedAt` fields to `Result`
- Sort results by `JobID`
- Add `context.Context` cancellation
- Add tests for empty jobs, invalid worker count, and one-result-per-job

---

[**Next ->** Project Tutorials And Next Steps](./05-project-tutorials-and-next-steps.md)  
[**<- Previous** Common Concurrency Mistakes](./03-common-concurrency-mistakes.md)
