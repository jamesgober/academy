<h1 align="center">
    <img width="99" alt="Go logo" src="../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../README.md) / [Go](../README.md) / [Reference](./README.md)

---

# Packages, Modules, And Layout Cheat Sheet

> Fast lookup for Go project structure, module boundaries, package names, and
> import rules.

## Related Lessons

- [Go Modules And Dependencies](../course/01-getting-started/05-go-modules-and-dependencies.md)
- [Packages And Imports](../course/03-packages-modules-and-project-layout/01-packages-and-imports.md)
- [Exported And Unexported Names](../course/03-packages-modules-and-project-layout/02-exported-and-unexported-names.md)
- [Structs And Methods In Go](../course/03-packages-modules-and-project-layout/03-structs-and-methods-in-go.md)
- [Project Layout And Module Boundaries](../course/03-packages-modules-and-project-layout/05-project-layout-and-module-boundaries.md)

---

## Core Terms

| Term | Meaning |
|---|---|
| file | one `.go` source file |
| package | files in one folder that compile together |
| module | dependency/project boundary defined by `go.mod` |
| `package main` | runnable application package |
| non-main package | reusable package |
| exported name | starts with uppercase, visible to other packages |
| unexported name | starts with lowercase, private to package |

---

## Module Example

```text
garage-app/
  go.mod
  main.go
  garage/
    status.go
```

`go.mod`:

```go
module example.com/garage-app

go 1.22
```

Import:

```go
import "example.com/garage-app/garage"
```

---

## Layout Choices

Tiny app:

```text
app/
  go.mod
  main.go
```

App with domain package:

```text
app/
  go.mod
  main.go
  orders/
    order.go
    order_test.go
```

App with private internals:

```text
app/
  go.mod
  main.go
  internal/
    config/
      config.go
```

---

## `internal`

Packages inside `internal` can only be imported by code inside the parent tree.

Use when:

- implementation should stay private
- external modules should not depend on the package

Do not use it everywhere. Beginners can wait until privacy matters.

---

## Package Naming

Prefer:

```text
garage
orders
config
worker
store
```

Avoid vague names:

```text
utils
helpers
common
misc
```

---

## Export Checklist

Export a name when another package needs it:

```go
func Status(v Vehicle) string
```

Keep helpers unexported:

```go
func speedBand(speed int) string
```

Risk notice:

```text
Exported names become part of your package's public API. Keep the API small.
```

---

## Architecture Smells

- giant `main.go`
- package named `utils`
- interface before there are multiple implementations
- circular imports
- package with too many exported names
- tests that require console input
- huge project layout copied into a tiny app

---

[Reference Index](./README.md) / [Go](../README.md) / [Home](../../../README.md)
