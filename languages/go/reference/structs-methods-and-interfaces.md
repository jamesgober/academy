<h1 align="center">
    <img width="99" alt="Go logo" src="../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../README.md) / [Go](../README.md) / [Reference](./README.md)

---

# Structs, Methods, And Interfaces

Use this page when you need a quick reminder of Go custom types. For the guided
lessons, see [Structs and Methods in Go](../course/03-packages-modules-and-project-layout/03-structs-and-methods-in-go.md) and [Interfaces in Plain Language](../course/03-packages-modules-and-project-layout/04-interfaces-in-plain-language.md).

## Structs

A struct groups related fields:

```go
type Vehicle struct {
    Plate string
    Kind  string
}
```

Create a value:

```go
vehicle := Vehicle{
    Plate: "ABC-123",
    Kind:  "car",
}
```

Use named fields for beginner code. They make the meaning obvious and survive
field reordering better than positional literals.

## Exported And Unexported Fields

```go
type Garage struct {
    capacity int
    vehicles []Vehicle
}
```

Lowercase fields are only accessible inside the package. This protects package
state.

Expose behavior through methods:

```go
func (g Garage) Count() int {
    return len(g.vehicles)
}
```

## Methods

A method is a function attached to a type:

```go
func (g Garage) Count() int {
    return len(g.vehicles)
}
```

The receiver is:

```go
(g Garage)
```

Read it as:

```text
this method belongs to Garage
inside the method, call the current value g
```

## Value Receiver Versus Pointer Receiver

Use a value receiver when the method only reads:

```go
func (g Garage) Count() int {
    return len(g.vehicles)
}
```

Use a pointer receiver when the method changes the value:

```go
func (g *Garage) Park(vehicle Vehicle) bool {
    if len(g.vehicles) >= g.capacity {
        return false
    }

    g.vehicles = append(g.vehicles, vehicle)
    return true
}
```

Beginner rule:

```text
mutates receiver -> pointer receiver
only reads receiver -> value receiver can be fine
```

For large structs, pointer receivers can avoid copying.

## Constructor-Style Functions

Go does not have class constructors. Use ordinary functions:

```go
func NewGarage(capacity int) Garage {
    return Garage{
        capacity: capacity,
        vehicles: []Vehicle{},
    }
}
```

Use `NewTypeName` when setup rules matter.

## Interfaces

An interface describes behavior:

```go
type Reporter interface {
    Report() string
}
```

Any type with `Report() string` satisfies it:

```go
func (g Garage) Report() string {
    return fmt.Sprintf("%d spaces used", g.Count())
}
```

There is no `implements` keyword. Satisfaction is implicit.

## Small Interfaces

Prefer tiny behavior-focused interfaces:

```go
type Saver interface {
    Save(text string) error
}
```

Avoid huge interfaces that force unrelated methods together.

## Risk Notices

- Do not export fields just because another package wants convenience.
- Do not create interfaces before there is a real boundary or test need.
- Do not use pointer receivers randomly; know whether mutation is required.
- Do not mix unrelated responsibilities into one struct.
- Be careful copying structs that contain locks, buffers, or large slices.

## Related Lessons

- [Structs and Methods in Go](../course/03-packages-modules-and-project-layout/03-structs-and-methods-in-go.md)
- [Interfaces in Plain Language](../course/03-packages-modules-and-project-layout/04-interfaces-in-plain-language.md)
- [Chapter 03 Checkpoint](../course/03-packages-modules-and-project-layout/06-chapter-03-checkpoint.md)

---

[Reference Index](./README.md) / [Go](../README.md) / [Home](../../../README.md)
