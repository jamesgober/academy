<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Go](../../README.md) · [Chapter 03](./README.md)

</div>

---

# Exported and Unexported Names

> In Go, capitalization affects visibility.

**You will learn:**
- What exported means
- Why uppercase and lowercase matter
- How to keep public APIs small

**Before this page, you should know:** [Packages and Imports](./01-packages-and-imports.md)

---

## The rule

If a name starts with an uppercase letter, it is exported.
If it starts with a lowercase letter, it is package-private.

```go
func PublicMessage() string { return "shown outside package" }
func privateMessage() string { return "used only inside package" }
```

## Why this matters

This is how Go decides what outside packages can use.
It is simple, but you must remember it.

> [!IMPORTANT]
> A public API should be smaller than the total code inside a package.

---

## Recap

- Uppercase starts exported names.
- Lowercase starts unexported names.
- Small public surfaces are easier to maintain.

## Try it yourself

Write one exported function and one unexported helper and explain why the split makes sense.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Packages and Imports](./01-packages-and-imports.md) | [Chapter 03](./README.md) · [Go](../../README.md) · [Home](../../../../README.md) | [Structs and Methods in Go →](./03-structs-and-methods-in-go.md) |

</div>
