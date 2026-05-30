<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 05](./README.md)

---

# Channels And Message Passing

> Channels let goroutines communicate by moving values. They are not magic
> queues; they are synchronization points with very specific behavior.

**You will learn:**
- How to create channels
- How send and receive syntax works
- Why unbuffered channels block
- When buffered channels help
- How to close and range over channels
- How `select` coordinates multiple channel operations

**Before this page, you should know:** [Goroutines In Plain Language](./01-goroutines-in-plain-language.md)

---

## Channel Mental Model

A channel carries values of one type:

```go
messages := make(chan string)
```

Plain meaning:

```text
messages is a channel that carries string values.
```

Send:

```go
messages <- "ready"
```

Receive:

```go
value := <-messages
```

Read the arrow as movement:

```text
messages <- "ready"  value goes into channel
value := <-messages  value comes out of channel
```

---

## First Working Example

```go
package main

import "fmt"

func main() {
    messages := make(chan string)

    go func() {
        messages <- "worker is done"
    }()

    message := <-messages
    fmt.Println(message)
}
```

Why the goroutine matters:

```text
main waits to receive.
worker sends.
receive completes.
```

If the same goroutine tries to send and receive in the wrong order, it can
deadlock.

---

## Unbuffered Channels Block

The default channel is unbuffered:

```go
messages := make(chan string)
```

Unbuffered send blocks until another goroutine receives.

Unbuffered receive blocks until another goroutine sends.

Visual model:

```text
Unbuffered channel:

sender ---- value ---- receiver

The handoff needs both sides.
```

This is why channels synchronize goroutines.

---

## Deadlock Example

```go
package main

func main() {
    messages := make(chan string)

    messages <- "ready"
}
```

This program blocks forever because no other goroutine receives the value.

Go reports:

```text
fatal error: all goroutines are asleep - deadlock!
```

Fix by adding a receiver goroutine or by receiving before the program exits.

---

## Buffered Channels

A buffered channel can hold a limited number of values without an immediate
receiver:

```go
messages := make(chan string, 2)

messages <- "first"
messages <- "second"

fmt.Println(<-messages)
fmt.Println(<-messages)
```

Capacity:

```text
2
```

Plain meaning:

```text
This channel can hold two strings before senders block.
```

Buffered channels are useful when you want a little breathing room between
producers and consumers.

Risk:

> Buffers can hide coordination problems. Do not add a buffer only to make a
> deadlock disappear.

---

## Closing Channels

Close a channel when senders are done sending:

```go
jobs := make(chan string)

go func() {
    defer close(jobs)

    jobs <- "a.txt"
    jobs <- "b.txt"
    jobs <- "c.txt"
}()

for job := range jobs {
    fmt.Println("processing", job)
}
```

Important:

```text
The sender closes the channel.
Receivers do not close channels they do not own.
```

Closing tells receivers:

```text
No more values are coming.
```

It does not mean:

```text
Please stop immediately.
```

---

## Receive With `ok`

```go
value, ok := <-jobs
```

If `ok` is `true`, a real value was received.

If `ok` is `false`, the channel is closed and drained.

Example:

```go
value, ok := <-jobs
if !ok {
    fmt.Println("no more jobs")
    return
}

fmt.Println(value)
```

Use `range` when you just want to process values until the channel closes.

---

## Directional Channels

Function parameters can say whether a channel is send-only or receive-only.

Send-only:

```go
func produce(out chan<- string) {
    out <- "job"
}
```

Receive-only:

```go
func consume(in <-chan string) {
    fmt.Println(<-in)
}
```

This documents intent and lets the compiler catch mistakes.

---

## `select`

`select` waits on multiple channel operations:

```go
select {
case message := <-messages:
    fmt.Println("message:", message)
case err := <-errors:
    fmt.Println("error:", err)
}
```

Whichever case is ready first runs.

Common timeout pattern:

```go
select {
case result := <-results:
    fmt.Println("result:", result)
case <-time.After(2 * time.Second):
    fmt.Println("timed out")
}
```

Use `select` when several things could happen next.

---

## Channel Ownership

Ask:

```text
Who creates the channel?
Who sends?
Who receives?
Who closes?
What happens if nobody sends?
What happens if nobody receives?
```

A clean channel design can be explained in one sentence:

```text
The producer goroutine sends filenames to jobs and closes jobs when the list is done.
Workers receive from jobs and send reports to results.
The coordinator receives reports until all workers finish.
```

If you cannot explain the ownership, the code is not ready.

---

## Mini Project: Status Channel

Build a program that:

- Starts one goroutine
- Sends three status messages
- Closes the channel
- Ranges over messages in `main`

Starter:

```go
package main

import "fmt"

func main() {
    statuses := make(chan string)

    go func() {
        defer close(statuses)

        statuses <- "starting"
        statuses <- "working"
        statuses <- "done"
    }()

    for status := range statuses {
        fmt.Println(status)
    }
}
```

Then change it:

- Make the channel buffered with capacity `1`
- Add a second producer
- Explain why closing is now trickier

Hint: multiple senders require coordination. One sender should not close a
channel while another sender might still send.

---

## Chapter Checkpoint

You should now be able to answer:

- What type does `make(chan string)` create?
- What does `messages <- "ready"` do?
- What does `<-messages` do?
- Why do unbuffered channels block?
- Who should close a channel?
- What does `for value := range ch` do?
- What does `value, ok := <-ch` tell you?
- When is `select` useful?

---

## Recap

- Channels move typed values between goroutines.
- Unbuffered channels synchronize sender and receiver.
- Buffered channels hold limited values.
- Senders close channels when no more values will be sent.
- Receivers can range until a channel closes.
- `select` coordinates multiple channel operations.

## Try It Yourself

Write a `produceJobs(out chan<- string)` function and a
`printJobs(in <-chan string)` function. Use directional channel parameters and
explain what each direction allows.

---

[**Next ->** Common Concurrency Mistakes](./03-common-concurrency-mistakes.md)  
[**<- Previous** Goroutines In Plain Language](./01-goroutines-in-plain-language.md)
