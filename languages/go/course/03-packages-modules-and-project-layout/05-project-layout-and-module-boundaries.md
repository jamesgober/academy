<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 03](./README.md)

---

# Project Layout And Module Boundaries

> Go project layout should make responsibility obvious. A simple layout you can
> explain beats a clever layout you copied without understanding.

**You will learn:**
- How small Go projects should be shaped
- What belongs in `main`
- What belongs in reusable packages
- When to create a package
- What `internal` means
- How module boundaries affect imports
- Which architecture smells to avoid early

**Before this page, you should know:** [Interfaces In Plain Language](./04-interfaces-in-plain-language.md)

---

## Start Boring

A tiny Go app can start as:

```text
hello/
  go.mod
  main.go
```

That is fine.

Do not create five folders before the program has five responsibilities.

---

## Add A Package When Responsibility Separates

When domain logic grows, split it:

```text
garage-app/
  go.mod
  main.go
  garage/
    vehicle.go
    status.go
    status_test.go
```

Meaning:

```text
main.go handles app startup and input/output.
garage package handles garage rules.
tests live beside the package they test.
```

---

## Example Module

`go.mod`:

```go
module example.com/garage-app

go 1.22
```

`garage/status.go`:

```go
package garage

type Vehicle struct {
    Plate string
    Speed int
}

func Status(v Vehicle) string {
    if v.Speed > 120 {
        return "warning"
    }

    return "ok"
}
```

`main.go`:

```go
package main

import (
    "fmt"

    "example.com/garage-app/garage"
)

func main() {
    vehicle := garage.Vehicle{
        Plate: "ABC-123",
        Speed: 90,
    }

    fmt.Println(garage.Status(vehicle))
}
```

Run:

```bash
go run .
```

Test:

```bash
go test ./...
```

---

## What Belongs In `main`

Package `main` should usually handle:

- command-line arguments
- reading user input
- printing output
- wiring packages together
- process exit behavior

Avoid putting all business rules in `main`.

Better:

```text
main asks garage package to calculate status.
garage package does not know about console prompts.
```

This makes the core logic easier to test.

---

## What Belongs In A Reusable Package

A reusable package should hold one clear responsibility:

```text
garage: vehicle status rules
config: load app configuration
worker: job processing
store: persistence
```

Good package signs:

- name is short and concrete
- exported names are few
- tests are easy to write
- package can be explained in one sentence

---

## The `internal` Folder

Go has a special folder name:

```text
internal
```

Example:

```text
garage-app/
  go.mod
  main.go
  internal/
    config/
      config.go
```

Packages inside `internal` can only be imported by code inside the parent tree.

Plain language:

```text
internal means this package is private to this module/project area.
```

Use it when you want to prevent other modules from importing implementation
details.

Beginners do not need `internal` immediately, but it is important to recognize.

---

## Package Names

Good:

```text
garage
config
worker
store
```

Weak:

```text
utils
helpers
common
misc
```

Why?

`utils` does not tell you what responsibility lives there.

If a package becomes `utils`, ask whether it should be split into clearer names.

---

## Export Boundaries

Export only what other packages need.

```go
func Status(v Vehicle) string
```

is exported.

```go
func speedBand(speed int) string
```

is unexported.

Beginner rule:

```text
Start unexported. Export when another package truly needs it.
```

---

## Common Layouts

Small app:

```text
app/
  go.mod
  main.go
```

Small app with domain package:

```text
app/
  go.mod
  main.go
  orders/
    order.go
    order_test.go
```

App with internal implementation:

```text
app/
  go.mod
  main.go
  internal/
    config/
      config.go
    store/
      file_store.go
```

Command with reusable package:

```text
app/
  go.mod
  cmd/
    app/
      main.go
  orders/
    order.go
```

Do not use `cmd/` just because you saw it online. Use it when you actually have
one or more commands to organize.

---

## Architecture Smells

Watch for:

- one giant `main.go`
- package named `utils`
- too many exported names
- interfaces created before there are multiple implementations
- packages that import each other in circles
- business logic that cannot be tested without console input
- folder structure copied from a large project into a tiny project

Go rewards simple boundaries.

---

## Mini Project: Garage Layout

Build:

```text
garage-app/
  go.mod
  main.go
  garage/
    vehicle.go
    status.go
    status_test.go
```

Rules:

- `garage.Vehicle` has `Plate` and `Speed`
- `garage.Status` returns `"ok"` or `"warning"`
- `main` prints status for two vehicles
- tests cover both statuses

Quality commands:

```bash
gofmt -w .
go test ./...
go run .
go mod tidy
```

Explain:

```text
Why is Status in garage instead of main?
Which names are exported?
Which names could stay unexported?
```

---

## Chapter Checkpoint

You should now be able to answer:

- When is one `main.go` enough?
- What belongs in package `main`?
- When should you create a reusable package?
- What does `internal` do?
- Why are `utils` packages suspicious?
- Why should you export fewer names?
- Why should domain logic avoid console input?

---

## Recap

- Start with simple layout.
- Add packages when responsibilities separate.
- `main` should orchestrate, not hold every rule.
- `internal` hides implementation packages from outside importers.
- Package names should describe responsibility.
- Export only what other packages need.

## Try It Yourself

Sketch a Go project for a todo CLI:

- where does `main` live?
- where do task rules live?
- where do tests live?
- what names need to be exported?

---

[**Next ->** Chapter 03 Checkpoint](./06-chapter-03-checkpoint.md)  
[**<- Previous** Interfaces In Plain Language](./04-interfaces-in-plain-language.md)
