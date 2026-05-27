<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Go](../../README.md) · [Chapter 02](./README.md)

</div>

---

# Functions, Parameters, and Returns

> Functions package a small job into a reusable named block.

**You will learn:**
- What a function is
- What parameters are
- How return values bring data back
- Common function patterns used in real Go code

**Before this page, you should know:** [Variables, Values, and Types](./01-variables-values-and-types.md)

---

## Basic function shape

```go
func greet(name string) string {
    return "Hello, " + name
}
```

Read it slowly:
- `func` starts a function definition
- `greet` is the function name
- `name string` is one parameter
- the last `string` means the function returns text

## Calling a function

```go
message := greet("James")
fmt.Println(message)
```

## Multiple parameters and return values

```go
func divide(total int, by int) (int, int) {
    return total / by, total % by
}
```

Go supports returning multiple values from one function.

## Variadic parameter (`...`)

```go
func sum(values ...int) int {
    total := 0
    for _, v := range values {
        total += v
    }
    return total
}
```

Use variadic parameters when a function should accept zero or more values.

## Error-return pattern

Go often returns `(value, error)` rather than throwing exceptions.

```go
func parsePort(raw string) (int, error) {
    return strconv.Atoi(raw)
}
```

This pattern is central to professional Go code.

## Parameters versus arguments

- **parameter**: the named input in the function definition
- **argument**: the actual value you pass when calling it

## Why functions matter

They stop repetition.
They make code easier to read.
They let you test small pieces of behavior.

## When to choose alternatives

- use plain functions for standalone behavior
- use methods when behavior is tightly tied to a type
- keep function signatures small when possible

> [!NOTE]
> A lot of beginner confusion disappears once you mentally translate a function
> into: input goes in, work happens, result comes out.

---

## Recap

- Functions define reusable behavior.
- Parameters describe inputs.
- Returns send results back.
- Go commonly uses multiple return values, including `error`.

## Try it yourself

Write a function named `double` that accepts an integer and returns twice its
value.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Variables, Values, and Types](./01-variables-values-and-types.md) | [Chapter 02](./README.md) · [Go](../../README.md) · [Home](../../../../README.md) | [Conditionals and Comparison →](./03-conditionals-and-comparison.md) |

</div>
