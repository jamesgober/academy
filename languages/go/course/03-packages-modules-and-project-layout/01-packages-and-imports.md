<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Go](../../README.md) · [Chapter 03](./README.md)

</div>

---

# Packages and Imports

> Packages let Go code be grouped by responsibility instead of becoming one giant file.

**You will learn:**
- What a package is
- How imports bring in code from other packages
- Why package boundaries matter for readability

**Before this page, you should know:** [Chapter 02](../02-core-language-basics/README.md)

---

## A package is a code group

A package is a set of Go files that belong together.
Usually those files live in one folder.

## Importing a package

```go
import "fmt"
```

That line makes the `fmt` package available in your file.

## Why imports matter

Imports make dependencies visible.
When you read the top of a Go file, you should quickly see what outside tools it uses.

> [!TIP]
> If a file imports many unrelated packages, it may be doing too many jobs.

---

## Recap

- Packages group related code.
- Imports make outside packages available.
- Visible dependencies improve readability.

## Try it yourself

Make one small package with a helper function, then import and call it from `main`.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Chapter 02](../02-core-language-basics/README.md) | [Chapter 03](./README.md) · [Go](../../README.md) · [Home](../../../../README.md) | [Exported and Unexported Names →](./02-exported-and-unexported-names.md) |

</div>
