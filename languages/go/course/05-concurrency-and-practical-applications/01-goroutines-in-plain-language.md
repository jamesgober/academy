<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 05](./README.md)

---

# Goroutines In Plain Language

A goroutine is a lightweight concurrent task managed by the Go runtime.

The keyword is tiny:

```go
go doWork()
```

The idea behind it is not tiny. When you start a goroutine, you are saying:

```text
start this function, but do not wait here for it to finish
```

## Concurrency Is Not Magic Speed

Concurrency means multiple tasks can make progress during the same overall time
period.

It does not automatically mean every program becomes faster.

Concurrency helps most when work involves waiting:

- network calls
- file I/O
- timers
- independent tasks
- background processing

Concurrency can hurt when it adds coordination complexity without solving a real
problem.

## First Example

```go
package main

import "fmt"

func say(message string) {
    fmt.Println(message)
}

func main() {
    go say("from goroutine")
    say("from main")
}
```

This program might print both lines, or it might only print:

```text
from main
```

Why? Because `main` can finish before the goroutine gets a chance to run.

That is the first beginner rule:

```text
starting a goroutine is not the same as waiting for it
```

## Visual Model

```text
main goroutine
  |
  | go say("from goroutine")
  |---------------------------> new goroutine
  |
  | say("from main")
  v
main may finish
```

If `main` exits, the program ends. It does not politely wait for every loose
goroutine.

## Waiting With `sync.WaitGroup`

Use `sync.WaitGroup` when you need to wait for a known number of goroutines.

```go
package main

import (
    "fmt"
    "sync"
)

func worker(id int, wg *sync.WaitGroup) {
    defer wg.Done()
    fmt.Println("worker", id, "done")
}

func main() {
    var wg sync.WaitGroup

    for id := 1; id <= 3; id++ {
        wg.Add(1)
        go worker(id, &wg)
    }

    wg.Wait()
    fmt.Println("all workers done")
}
```

Read it slowly:

```text
wg.Add(1)   one more goroutine must finish
go worker   start the goroutine
defer Done  mark this worker done when it returns
wg.Wait()   block until the count returns to zero
```

`defer wg.Done()` is common because it runs when the worker function exits.

## Do Not Use Sleep As Real Coordination

You may see beginner demos like this:

```go
time.Sleep(time.Second)
```

Sleep can make a tiny demo appear to work, but it is not reliable coordination.

Bad mental model:

```text
wait one second and hope the goroutine finished
```

Better mental model:

```text
use WaitGroup or channels to know the goroutine finished
```

Channels are covered next.

## Loop Variables And Goroutines

Be careful when starting goroutines inside loops.

This is safe because the worker receives `id` as an argument:

```go
for id := 1; id <= 3; id++ {
    wg.Add(1)
    go worker(id, &wg)
}
```

Passing the value into the function makes each goroutine get its own `id`.

If a goroutine accidentally shares a changing loop variable, the output can be
confusing.

## When To Use A Goroutine

Good reasons:

- run independent work concurrently
- keep a server responsive while handling requests
- process jobs in the background
- wait on several slow operations at once

Weak reasons:

- "goroutines are cool"
- "I want this tiny calculation to look advanced"
- "I do not understand why this code is slow"

Concurrency is a tool, not decoration.

## Common Beginner Mistakes

### Forgetting To Wait

```go
go doWork()
```

This starts work. It does not wait for work.

### Writing To Shared Data Without A Plan

If multiple goroutines change the same variable, you need coordination.

Later lessons cover channels, mutexes, and worker patterns.

### Starting Too Many Goroutines

Goroutines are lightweight, not free. If you start one per item for a million
items, you may create a new problem.

Worker pools help limit concurrency.

## Practice

Write a program with three workers:

```go
func worker(id int, wg *sync.WaitGroup) {
    defer wg.Done()
    fmt.Println("worker", id, "finished")
}
```

Start workers `1`, `2`, and `3` in a loop. Use `sync.WaitGroup` so `main` waits
for all of them.

Expected output order may vary:

```text
worker 2 finished
worker 1 finished
worker 3 finished
all workers finished
```

The order can vary because concurrent tasks are scheduled by the runtime.

## What You Should Be Able To Explain

Before moving on, make sure you can say:

- what the `go` keyword does
- why `main` can finish before a goroutine
- what `sync.WaitGroup` tracks
- why `Done` usually appears with `defer`
- why output order can vary
- why concurrency adds coordination problems

---

[**Next ->** Channels and Message Passing](./02-channels-and-message-passing.md)  
[**<- Previous** Chapter 04](../04-testing-and-tooling/README.md)
