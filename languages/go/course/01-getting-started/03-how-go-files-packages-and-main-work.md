<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Go](../../README.md) · [Chapter 01](./README.md)

</div>

---

# How Go Files, Packages, and `main` Work

> Before you learn more syntax, make the file layout make sense.

**You will learn:**
- What a package is in Go
- Why multiple files can belong to one package
- How application code differs from reusable package code

**Before this page, you should know:** [Your First Go Program](./02-your-first-go-program.md)

---

## A package is a group of Go files

In Go, files are grouped into **packages**.
Files in the same folder usually belong to the same package.

That means this is normal:

```text
hello-go/
├── main.go
├── message.go
└── printer.go
```

All three files can say:

```go
package main
```

and they still belong to one program.

## Why this matters

Beginners sometimes think one file equals one program part.
Go does not force that model.
You can split a program across files for clarity while keeping one package.

## `package main` versus other packages

- `package main` builds a runnable program.
- other package names usually hold reusable code.

Example reusable package:

```go
package greeting

func Message() string {
    return "Hello from a reusable package"
}
```

## The mental model

Think of it this way:

- a **file** is one page of code
- a **package** is a folder-level unit of code that works together
- `main` is the package Go can run as an application

> [!TIP]
> If a beginner gets lost in Go structure, ask: "What package is this file in, and is this package supposed to be runnable or reusable?"

---

## Recap

- Files are grouped into packages.
- `package main` means runnable application code.
- Non-`main` packages are usually for reusable logic.

## Try it yourself

Create a second file in the same folder that defines a helper function, then
call it from `main.go`.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Your First Go Program](./02-your-first-go-program.md) | [Chapter 01](./README.md) · [Go](../../README.md) · [Home](../../../../README.md) | [Go Command Workflow →](./04-go-command-workflow.md) |

</div>
