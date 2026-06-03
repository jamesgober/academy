<h1 align="center">
    <img width="99" alt="Go logo" src="../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../README.md) / [Go](../README.md) / [Reference](./README.md)

---

# Go Concurrency Cheat Sheet

Use this page when you need to remember goroutines, channels, waiting, and
shutdown basics. For the guided lessons, start with [Goroutines in Plain Language](../course/05-concurrency-and-practical-applications/01-goroutines-in-plain-language.md).

## Goroutine

Start a concurrent task:

```go
go doWork()
```

Meaning:

```text
start doWork in a goroutine and continue immediately
```

Starting is not waiting. If `main` exits, the program exits.

## WaitGroup

Wait for a known number of goroutines:

```go
var wg sync.WaitGroup

for id := 1; id <= 3; id++ {
    wg.Add(1)
    go func(id int) {
        defer wg.Done()
        fmt.Println("worker", id)
    }(id)
}

wg.Wait()
```

Key calls:

```text
Add(1)  one more task to wait for
Done()  one task finished
Wait()  block until all tasks are done
```

## Channels

A channel sends typed values between goroutines:

```go
messages := make(chan string)
```

Send:

```go
messages <- "done"
```

Receive:

```go
message := <-messages
```

## Buffered Channels

Unbuffered channel:

```go
ch := make(chan int)
```

Send waits until another goroutine receives.

Buffered channel:

```go
ch := make(chan int, 3)
```

Up to three values can wait in the channel before sends block.

## Closing Channels

Close a channel to tell receivers no more values are coming:

```go
close(jobs)
```

Range over a channel:

```go
for job := range jobs {
    process(job)
}
```

Only the sender should close a channel. Receivers should not close channels they
do not own.

## `select`

`select` waits on multiple channel operations:

```go
select {
case result := <-results:
    fmt.Println(result)
case <-done:
    fmt.Println("cancelled")
}
```

Use it when a goroutine may receive from more than one source.

## Worker Pool Shape

```text
jobs channel -> fixed number of workers -> results channel
```

This limits concurrency instead of starting unlimited goroutines.

## Race Risk

This is risky:

```go
count := 0

go func() {
    count++
}()

go func() {
    count++
}()
```

Multiple goroutines are writing shared data. Use channels, a mutex, or another
clear coordination strategy.

Run concurrent tests with:

```bash
go test -race ./...
```

## Diagnostic Questions

Ask these before adding concurrency:

- What starts each goroutine?
- What tells it to stop?
- How does `main` wait?
- Where do errors go?
- Who closes each channel?
- Is shared data protected?
- Would simple single-threaded code be clearer?

## Risk Notices

- Concurrency is not automatically faster.
- `time.Sleep` is not reliable coordination.
- Unbounded goroutine creation can exhaust resources.
- Sending on a closed channel panics.
- Reading from a closed channel returns the zero value, plus `ok == false` when
  using comma-ok receive.
- Data races can produce bugs that appear only sometimes.

## Related Lessons

- [Goroutines in Plain Language](../course/05-concurrency-and-practical-applications/01-goroutines-in-plain-language.md)
- [Channels and Message Passing](../course/05-concurrency-and-practical-applications/02-channels-and-message-passing.md)
- [Common Concurrency Mistakes](../course/05-concurrency-and-practical-applications/03-common-concurrency-mistakes.md)
- [A Small Worker Pattern](../course/05-concurrency-and-practical-applications/04-a-small-worker-pattern.md)

---

[Reference Index](./README.md) / [Go](../README.md) / [Home](../../../README.md)
