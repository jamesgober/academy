<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Go](../../README.md) · [Chapter 05](./README.md)

</div>

---

# Channels and Message Passing

> Channels let goroutines communicate without every part of the program touching the same data directly.

**You will learn:**
- What a channel is
- Why message passing can be safer than shared state
- How to read send and receive syntax

**Before this page, you should know:** [Goroutines in Plain Language](./01-goroutines-in-plain-language.md)

---

## Channel example

```go
messages := make(chan string)
messages <- "ready"
value := <-messages
```

- `messages <- "ready"` sends a value
- `value := <-messages` receives a value

## Why channels matter

Channels help coordinate work.
They make data flow explicit.
They reduce some shared-state problems.

> [!TIP]
> When learning channels, read the arrows as actual movement: value goes in, value comes out.

---

## Recap

- Channels move values between goroutines.
- Send and receive use arrow syntax.
- Message passing helps coordination stay explicit.

## Try it yourself

Sketch a tiny program where one goroutine sends a status string to another.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Goroutines in Plain Language](./01-goroutines-in-plain-language.md) | [Chapter 05](./README.md) · [Go](../../README.md) · [Home](../../../../README.md) | [Common Concurrency Mistakes →](./03-common-concurrency-mistakes.md) |

</div>
