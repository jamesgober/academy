<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Go](../../README.md) · [Chapter 01](./README.md)

</div>

---

# Go Modules and Dependencies

> A Go module is the unit Go uses for dependency management and project identity.

**You will learn:**
- What a Go module is
- How `go mod init` starts a project
- How dependencies are added and tracked

**Before this page, you should know:** [Go Command Workflow](./04-go-command-workflow.md)

---

## Start a module

Inside your project folder, run:

```bash
go mod init example/hello-go
```

This creates a `go.mod` file.

`go.mod` tells Go:
- the module name
- the Go version
- the dependencies your project needs

## Why modules matter

Without a module, larger projects become difficult to organize.
The module gives Go a stable project identity.

## Adding a dependency

Many dependencies are added automatically the first time you import and use
something, then run a Go command that resolves modules.

Common cleanup command:

```bash
go mod tidy
```

`go mod tidy` removes unused dependencies and adds missing ones.

> [!TIP]
> Run `go mod tidy` after meaningful dependency changes so `go.mod` and `go.sum` stay honest.

## Beginner-safe rule

Do not add libraries just because they look popular.
Stay with the standard library until a real problem appears.

---

## Recap

- A Go module defines the project boundary for dependencies.
- `go mod init` starts module-based project management.
- `go mod tidy` keeps dependency files clean.

## Try it yourself

Run `go mod init` in your practice project and inspect the generated `go.mod`
file line by line.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Go Command Workflow](./04-go-command-workflow.md) | [Chapter 01](./README.md) · [Go](../../README.md) · [Home](../../../../README.md) | [Testing, Formatting, and Documentation Basics →](./06-testing-formatting-and-documentation-basics.md) |

</div>
