<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Go](../../README.md) · [Chapter 05](./README.md)

</div>

---

# Goroutines in Plain Language

> A goroutine is a lightweight concurrent task managed by the Go runtime.

**You will learn:**
- What a goroutine is
- Why concurrency is not the same thing as magic speed
- How to read the `go` keyword in code

**Before this page, you should know:** [Chapter 04](../04-testing-and-tooling/README.md)

---

## Basic idea

```go
go doWork()
```

That `go` keyword starts `doWork` as a goroutine.
It may run alongside other work.

## Beginner mental model

Think of a goroutine as "start this task and let the runtime schedule it."
Not everything should become a goroutine.
Only use concurrency when the problem benefits from it.

> [!IMPORTANT]
> Concurrency can improve structure and responsiveness, but it also adds coordination problems.

---

## Recap

- Goroutines are lightweight concurrent tasks.
- The `go` keyword starts one.
- Concurrency adds power and complexity together.

## Try it yourself

Write one function that prints a short message and start it as a goroutine.
Then explain why the main program might end before the goroutine finishes.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Chapter 04](../04-testing-and-tooling/README.md) | [Chapter 05](./README.md) · [Go](../../README.md) · [Home](../../../../README.md) | [Channels and Message Passing →](./02-channels-and-message-passing.md) |

</div>
