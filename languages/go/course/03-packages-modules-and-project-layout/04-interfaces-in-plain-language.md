<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 03](./README.md)

---

# Interfaces In Plain Language

An interface describes behavior a value can provide.

In Go, types do not have to announce that they implement an interface. If the
methods match, the type satisfies the interface.

That is called implicit satisfaction.

## A Small Interface

```go
type Speaker interface {
    Speak() string
}
```

Any type with this method satisfies `Speaker`:

```go
Speak() string
```

## A Type That Satisfies It

```go
type Greeter struct {
    Name string
}

func (g Greeter) Speak() string {
    return "hello, " + g.Name
}
```

`Greeter` satisfies `Speaker` because it has the right method.

No `implements Speaker` line is required.

## Complete Example

```go
package main

import "fmt"

type Speaker interface {
    Speak() string
}

type Greeter struct {
    Name string
}

func (g Greeter) Speak() string {
    return "hello, " + g.Name
}

func PrintSpeech(s Speaker) {
    fmt.Println(s.Speak())
}

func main() {
    greeter := Greeter{Name: "Ada"}
    PrintSpeech(greeter)
}
```

Output:

```text
hello, Ada
```

Visual model:

```text
PrintSpeech needs: anything with Speak() string
Greeter has:       Speak() string
Therefore:         Greeter can be passed to PrintSpeech
```

## Interfaces Are About The Caller

A beginner mistake is creating interfaces too early.

Bad smell:

```go
type Garage interface {
    Park(car string)
    Remove(car string)
    Count() int
    PrintReport()
    SaveToDisk(path string) error
}
```

That interface is large and unfocused.

Better:

```go
type Reporter interface {
    Report() string
}
```

Small interfaces are easier to satisfy, test, and understand.

## Realistic Example: Reporting

```go
package main

import "fmt"

type Reporter interface {
    Report() string
}

type Garage struct {
    Cars     int
    Capacity int
}

func (g Garage) Report() string {
    return fmt.Sprintf("%d of %d spaces used", g.Cars, g.Capacity)
}

type Sensor struct {
    Name  string
    Value int
}

func (s Sensor) Report() string {
    return fmt.Sprintf("%s=%d", s.Name, s.Value)
}

func PrintReport(r Reporter) {
    fmt.Println(r.Report())
}

func main() {
    PrintReport(Garage{Cars: 2, Capacity: 5})
    PrintReport(Sensor{Name: "temperature", Value: 72})
}
```

Both `Garage` and `Sensor` satisfy `Reporter` because both have:

```go
Report() string
```

## Interfaces And Testing

Interfaces are useful at boundaries.

Example:

```go
type Saver interface {
    Save(text string) error
}
```

Production code might save to a file.

Test code might use a fake saver that stores text in memory.

The function using `Saver` does not need to know which one it received.

## Accept Interfaces, Return Structs

A common Go guideline:

```text
accept interfaces, return concrete types
```

Meaning:

- function parameters can be interfaces when you only need behavior
- constructors usually return concrete structs

Example:

```go
type Writer interface {
    Write(text string) error
}

func PrintSummary(w Writer, text string) error {
    return w.Write(text)
}
```

`PrintSummary` does not care whether `w` writes to a file, terminal, buffer, or
test fake.

## Common Beginner Mistakes

### Creating Interfaces Before There Are Multiple Uses

If only one type exists and no boundary needs flexibility, use the concrete
type first.

Add an interface when it solves a real problem.

### Making Interfaces Too Large

Prefer:

```go
type Reader interface {
    Read() string
}
```

over:

```go
type Everything interface {
    Read() string
    Write(string) error
    Save(string) error
    Print()
}
```

### Putting Interfaces In The Wrong Package

Often, the package that uses the behavior should define the interface.

Do not force a package to export interfaces just because its structs exist.

## Practice

Define:

```go
type Runner interface {
    Run() string
}
```

Then create two structs:

```go
type Human struct {
    Name string
}

type Robot struct {
    ID string
}
```

Give both a `Run() string` method, then write:

```go
func PrintRun(r Runner) {
    fmt.Println(r.Run())
}
```

Call it with one `Human` and one `Robot`.

---

[**Next ->** Project Layout and Module Boundaries](./05-project-layout-and-module-boundaries.md)  
[**<- Previous** Structs and Methods in Go](./03-structs-and-methods-in-go.md)
