<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Go](../../README.md) · [Chapter 03](./README.md)

</div>

---

# Structs and Methods in Go

> Structs group related data, and methods attach behavior to that data.

**You will learn:**
- What a struct is
- What a method is
- How data and behavior can stay close together without becoming confusing

**Before this page, you should know:** [Exported and Unexported Names](./02-exported-and-unexported-names.md)

---

## Struct example

```go
type Car struct {
    Brand string
    Speed int
}
```

A struct is a custom data shape.

## Method example

```go
func (c Car) Summary() string {
    return c.Brand
}
```

A method is a function attached to a type.

## Beginner mental model

- struct = grouped data
- method = behavior tied to that data

> [!NOTE]
> Go supports methods without forcing you into heavy class-style thinking.

---

## Recap

- Structs define custom grouped data.
- Methods attach useful behavior.
- The pairing helps code feel organized.

## Try it yourself

Define a `Book` struct and add a method that returns a one-line summary.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Exported and Unexported Names](./02-exported-and-unexported-names.md) | [Chapter 03](./README.md) · [Go](../../README.md) · [Home](../../../../README.md) | [Interfaces in Plain Language →](./04-interfaces-in-plain-language.md) |

</div>
