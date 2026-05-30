<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 02](./README.md)

---

# Variables, Values, and Types

> Variables give names to data so the program can use it again later.

**You will learn:**
- What a variable is
- How Go stores values of different types
- Why types matter even when code looks simple

**Before this page, you should know:** [Chapter 01](../01-getting-started/README.md)

---

## A variable in plain language

A variable is a named place where your program keeps a value.

```go
var name string = "James"
age := 30
```

- `name` stores text
- `age` stores a number

## Why Go cares about type

Go wants to know what kind of data each value is.
That is its **type**.

Common beginner types:
- `string` for text
- `int` for whole numbers
- `bool` for true/false
- `float64` for decimal numbers

## Two common styles

```go
var language string = "Go"
framework := "none yet"
```

Use `var` when being explicit helps learning.
Use `:=` when Go can infer the type clearly.

> [!TIP]
> Beginners should read `:=` as "create a new variable with the value on the right."

## A common mistake

This does not work:

```go
var score int = "ten"
```

Why?
Because the value is text but the variable expects a number.

---

## Recap

- Variables name values.
- Values have types.
- Types help Go catch mistakes early.

## Try it yourself

Create three variables: one string, one integer, and one boolean. Print all
three with `fmt.Println`.

---

[**Next ->** Functions, Parameters, and Returns](./02-functions-parameters-and-returns.md)
[**<- Previous** Chapter 01](../01-getting-started/README.md)


