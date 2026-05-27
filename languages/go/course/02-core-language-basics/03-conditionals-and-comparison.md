<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Go](../../README.md) · [Chapter 02](./README.md)

</div>

---

# Conditionals and Comparison

> Programs make decisions by checking conditions.

**You will learn:**
- How `if` works in Go
- How comparison operators read
- How to think about true and false paths
- How to use `else if`, nested conditionals, and `switch`

**Before this page, you should know:** [Functions, Parameters, and Returns](./02-functions-parameters-and-returns.md)

---

## Basic `if`

```go
age := 20

if age >= 18 {
    fmt.Println("adult")
}
```

The block runs only when the condition is true.

## Common comparisons

- `==` equal to
- `!=` not equal to
- `>` greater than
- `<` less than
- `>=` greater than or equal to
- `<=` less than or equal to

## `if` and `else`

```go
if score >= 50 {
    fmt.Println("pass")
} else {
    fmt.Println("fail")
}
```

## `if` without `else`

This is normal when you only need to handle one condition:

```go
if user == nil {
    return
}
```

## `else if` chain

```go
if score >= 90 {
    fmt.Println("A")
} else if score >= 80 {
    fmt.Println("B")
} else {
    fmt.Println("C or below")
}
```

## Nested conditionals

```go
if online {
    if admin {
        fmt.Println("show admin panel")
    }
}
```

Use nested blocks carefully; too much nesting can hurt readability.

## `switch` as a cleaner alternative

```go
switch role {
case "owner":
    fmt.Println("full access")
case "editor":
    fmt.Println("limited access")
default:
    fmt.Println("read only")
}
```

`switch` is often easier to read than long `else if` chains.

## About ternary operators

Go does not have a ternary operator like `condition ? a : b`.
Use a normal `if` block for this choice.

## Mental model

A conditional asks one question:
"Which path should the program take right now?"

> [!TIP]
> When a beginner gets lost in a conditional, say the condition out loud in plain English first.

---

## Recap

- `if` chooses code paths.
- Comparisons produce true or false.
- `else` handles the other path.

## Try it yourself

Write a small program that prints `warm` if the temperature is above `20` and
`cold` otherwise.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Functions, Parameters, and Returns](./02-functions-parameters-and-returns.md) | [Chapter 02](./README.md) · [Go](../../README.md) · [Home](../../../../README.md) | [Loops and Repetition in Go →](./04-loops-and-repetition-in-go.md) |

</div>
