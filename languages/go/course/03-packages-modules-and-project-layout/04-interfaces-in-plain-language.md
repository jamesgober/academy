<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 03](./README.md)

---

# Interfaces in Plain Language

> An interface describes behavior a value can provide.

**You will learn:**
- What an interface is
- Why Go uses behavior-based design
- How interfaces reduce coupling when used carefully

**Before this page, you should know:** [Structs and Methods in Go](./03-structs-and-methods-in-go.md)

---

## Interface example

```go
type Speaker interface {
    Speak() string
}
```

Any type with a `Speak() string` method satisfies this interface.

## Why this matters

Go often cares more about what a value can do than what it is called.
That makes code flexible.

## Beginner-safe rule

Do not create interfaces just because you can.
Create them when they help separate responsibilities or testing boundaries.

> [!TIP]
> Small interfaces are usually better than large ones.

---

## Recap

- Interfaces describe behavior.
- Types satisfy interfaces by implementing the right methods.
- Interfaces help decouple code when used intentionally.

## Try it yourself

Define a `Runner` interface and two structs that satisfy it in different ways.

---

[**Next ->** Project Layout and Module Boundaries](./05-project-layout-and-module-boundaries.md)
[**<- Previous** Structs and Methods in Go](./03-structs-and-methods-in-go.md)


