<h1 align="center">
    <img width="99" alt="Go logo" src="../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../README.md) / [Go](../README.md) / [Reference](./README.md)

---

# Core Language Cheat Sheet

> Fast lookup for early Go syntax, containers, and beginner mental models.

## Related Lessons

- [Variables, Values, And Types](../course/02-core-language-basics/01-variables-values-and-types.md)
- [Functions, Parameters, And Returns](../course/02-core-language-basics/02-functions-parameters-and-returns.md)
- [Conditionals And Comparison](../course/02-core-language-basics/03-conditionals-and-comparison.md)
- [Loops And Repetition In Go](../course/02-core-language-basics/04-loops-and-repetition-in-go.md)
- [Arrays, Slices, And Maps](../course/02-core-language-basics/05-arrays-slices-and-maps-in-plain-language.md)

---

## Common Types

| Type | Meaning | Notice |
|---|---|---|
| `string` | text | immutable byte sequence |
| `int` | whole number | platform-sized |
| `bool` | `true` / `false` | no truthy/falsy values |
| `float64` | floating-point number | not exact decimal money |
| `byte` | alias for `uint8` | often raw bytes |
| `rune` | alias for `int32` | often Unicode code point |

Use explicit widths when boundaries matter:

```go
var small int32
var id int64
var b byte
```

---

## Variables

```go
var name string = "Ada"
age := 36
```

Use `:=` inside functions when the type is obvious.

Use `var` when:

- declaring package-level variables
- declaring zero-value variables
- making the type clearer

---

## Slices

Create:

```go
names := []string{"Ana", "Bo"}
```

Append:

```go
names = append(names, "Cam")
```

Append another slice:

```go
more := []string{"Dee", "Eli"}
names = append(names, more...)
```

Slice:

```go
middle := names[1:3]
```

Notice:

```text
append returns the updated slice.
slices can share backing storage.
```

---

## Maps

Create:

```go
scores := map[string]int{
    "Ana": 10,
}
```

Write:

```go
scores["Bo"] = 20
```

Safe lookup:

```go
score, ok := scores["Cam"]
if !ok {
    fmt.Println("missing score")
}
```

Delete:

```go
delete(scores, "Ana")
```

Notice:

```text
Writing to a nil map panics.
Map iteration order is not guaranteed.
```

---

## Loops

Go has one loop keyword:

```go
for i := 0; i < 3; i++ {
    fmt.Println(i)
}
```

Range over slice:

```go
for index, name := range names {
    fmt.Println(index, name)
}
```

Ignore index:

```go
for _, name := range names {
    fmt.Println(name)
}
```

---

## Functions

```go
func Add(left int, right int) int {
    return left + right
}
```

Multiple returns:

```go
func Divide(left int, right int) (int, bool) {
    if right == 0 {
        return 0, false
    }

    return left / right, true
}
```

---

## Risk Notices

- Go does not have truthy/falsy values; conditions must be `bool`.
- `append` result must be assigned.
- Slices can share storage after slicing.
- Missing map keys return zero values unless you use comma-ok.
- Map order is not stable.
- `float64` is not a money type.

---

[Reference Index](./README.md) / [Go](../README.md) / [Home](../../../README.md)
