<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 02](./README.md)

---

# Chapter 02 Checkpoint

This checkpoint confirms you can read and write small Go programs before
packages, modules, and project layout become the focus.

You will build a garage check-in program that uses:

- variables
- functions
- `if` and `else`
- `for` loops
- slices
- maps
- readable output

## Program Goal

Expected output:

```text
Garage check-in
---------------
Driver: Ada
Roadster is ready
Van needs inspection
Truck is ready
Ready vehicles: 2 of 3
```

## Step 1: Start The Program

Create `main.go`:

```go
package main

import "fmt"

func main() {
    driver := "Ada"
    vehicles := []string{"Roadster", "Van", "Truck"}

    fmt.Println("Garage check-in")
    fmt.Println("---------------")
    fmt.Println("Driver:", driver)
    fmt.Println("Vehicle count:", len(vehicles))
}
```

Run:

```bash
go run .
```

## Step 2: Add A Map For Status Lookup

Add this inside `main`:

```go
readiness := map[string]bool{
    "Roadster": true,
    "Van":      false,
    "Truck":    true,
}
```

Use a slice for ordered repeated items:

```go
vehicles := []string{"Roadster", "Van", "Truck"}
```

Use a map for lookup by key:

```go
readiness["Roadster"]
```

Visual model:

```text
vehicles slice controls order:
Roadster -> Van -> Truck

readiness map answers a question:
Roadster -> true
Van      -> false
Truck    -> true
```

## Step 3: Write A Function

Add this above `main`:

```go
func describeVehicle(name string, ready bool) string {
    if ready {
        return name + " is ready"
    }

    return name + " needs inspection"
}
```

The function receives:

- a vehicle name
- whether it is ready

It returns:

- one readable sentence

## Step 4: Loop Through The Vehicles

Inside `main`, add:

```go
readyCount := 0

for _, vehicle := range vehicles {
    ready := readiness[vehicle]

    if ready {
        readyCount++
    }

    fmt.Println(describeVehicle(vehicle, ready))
}
```

Read the loop:

```text
for each vehicle in vehicles:
    look up readiness
    count it if ready
    print the description
```

The blank identifier `_` ignores the index because this loop only needs the
vehicle name.

## Step 5: Print The Summary

Add:

```go
fmt.Printf("Ready vehicles: %d of %d\n", readyCount, len(vehicles))
```

`fmt.Printf` lets you format values into a string:

```text
%d -> integer
\n -> newline
```

## Complete Version

```go
package main

import "fmt"

func describeVehicle(name string, ready bool) string {
    if ready {
        return name + " is ready"
    }

    return name + " needs inspection"
}

func main() {
    driver := "Ada"
    vehicles := []string{"Roadster", "Van", "Truck"}
    readiness := map[string]bool{
        "Roadster": true,
        "Van":      false,
        "Truck":    true,
    }

    fmt.Println("Garage check-in")
    fmt.Println("---------------")
    fmt.Println("Driver:", driver)

    readyCount := 0

    for _, vehicle := range vehicles {
        ready := readiness[vehicle]

        if ready {
            readyCount++
        }

        fmt.Println(describeVehicle(vehicle, ready))
    }

    fmt.Printf("Ready vehicles: %d of %d\n", readyCount, len(vehicles))
}
```

## Must-Be-Able Checklist

You are ready for Chapter 03 when you can explain:

- why `driver` is a string
- why `vehicles` is a slice
- why `readiness` is a map
- how `range` loops over a slice
- why `_` is used when the index is ignored
- how `readyCount++` updates a count
- what `describeVehicle` receives
- what `describeVehicle` returns
- what `%d` means in `fmt.Printf`

## Stretch Practice

Make one change at a time:

- Add a fourth vehicle.
- Add a vehicle to the slice but not the map, then observe the default `false`.
- Use the comma-ok map lookup to detect missing status.
- Add a `driverGreeting` function.
- Print `All vehicles ready` when every vehicle is ready.

## Sign-Off Question

Why is this better than three separate variables named `vehicle1`, `vehicle2`,
and `vehicle3`?

Your answer should mention that slices and loops let the program grow without
copying the same code again and again.

---

[**Next ->** Chapter 03](../03-packages-modules-and-project-layout/README.md)  
[**<- Previous** Arrays, Slices, and Maps in Plain Language](./05-arrays-slices-and-maps-in-plain-language.md)
