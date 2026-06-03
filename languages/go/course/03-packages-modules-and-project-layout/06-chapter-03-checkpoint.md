<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 03](./README.md)

---

# Chapter 03 Checkpoint

This checkpoint confirms you can organize Go code without turning it into a
maze.

You will design a small vehicle tracker with:

- one executable `main` package
- one reusable `garage` package
- exported and unexported names
- a struct with methods
- one small interface at a behavior boundary

## Target Layout

```text
vehicle-tracker/
  go.mod
  main.go
  garage/
    garage.go
```

Create it:

```bash
mkdir vehicle-tracker
cd vehicle-tracker
go mod init example.com/vehicle-tracker
mkdir garage
```

## Step 1: Build The Garage Package

Create `garage/garage.go`:

```go
package garage

import "fmt"

type Vehicle struct {
    Plate string
    Kind  string
}

type Garage struct {
    capacity int
    vehicles []Vehicle
}

func NewGarage(capacity int) Garage {
    return Garage{
        capacity: capacity,
        vehicles: []Vehicle{},
    }
}

func (g *Garage) Park(vehicle Vehicle) bool {
    if len(g.vehicles) >= g.capacity {
        return false
    }

    g.vehicles = append(g.vehicles, vehicle)
    return true
}

func (g Garage) Count() int {
    return len(g.vehicles)
}

func (g Garage) Report() string {
    return fmt.Sprintf("%d of %d spaces used", g.Count(), g.capacity)
}
```

Important design choices:

- `Vehicle` is exported because `main` needs to create vehicles.
- `Garage` is exported because `main` needs a garage value.
- `capacity` and `vehicles` are unexported so callers cannot corrupt the state.
- `NewGarage` is exported because callers need a clean way to create a garage.
- `Park` uses a pointer receiver because it changes the garage.
- `Count` and `Report` use value receivers because they only read.

## Step 2: Build `main.go`

Create `main.go`:

```go
package main

import (
    "fmt"

    "example.com/vehicle-tracker/garage"
)

type Reporter interface {
    Report() string
}

func PrintReport(r Reporter) {
    fmt.Println(r.Report())
}

func main() {
    g := garage.NewGarage(2)

    first := garage.Vehicle{Plate: "ABC-123", Kind: "car"}
    second := garage.Vehicle{Plate: "VAN-900", Kind: "van"}
    third := garage.Vehicle{Plate: "TRK-777", Kind: "truck"}

    fmt.Println("park first:", g.Park(first))
    fmt.Println("park second:", g.Park(second))
    fmt.Println("park third:", g.Park(third))

    PrintReport(g)
}
```

Run:

```bash
go run .
```

Expected output:

```text
park first: true
park second: true
park third: false
2 of 2 spaces used
```

## Why The Interface Lives In `main`

`Reporter` is defined in `main` because `main` is the package that needs the
behavior boundary.

The `garage` package does not need to know that some caller wants a `Reporter`.
It simply provides a `Report() string` method.

This is a Go-friendly habit:

```text
define small interfaces where they are used
```

## Must-Be-Able Checklist

You are ready for Chapter 04 when you can explain:

- why `main.go` uses `package main`
- why `garage/garage.go` uses `package garage`
- why the import path includes the module path
- why `Vehicle` and `Garage` are exported
- why `capacity` and `vehicles` are unexported
- why `Park` has a pointer receiver
- why `Count` can use a value receiver
- why `Reporter` only asks for `Report() string`
- why a small interface is better than a large one here

## Stretch Practice

Make one change at a time:

- Add an exported `Capacity()` method.
- Add an exported `IsFull()` method.
- Add an unexported helper named `hasSpace`.
- Add a `RemoveByPlate` method.
- Move `Reporter` into another package only after you can justify why more than
  one caller needs it.

## Reviewer Checklist

Before calling this done:

- Run `gofmt -w .`
- Run `go run .`
- Check that exported names are intentional.
- Check that unexported fields protect package state.
- Check that the interface is small and behavior-focused.

---

[**Next ->** Chapter 04](../04-testing-and-tooling/README.md)  
[**<- Previous** Project Layout and Module Boundaries](./05-project-layout-and-module-boundaries.md)
