<h1 align="center">
    <img width="99" alt="Go logo" src="../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../README.md) / [Go](../README.md) / [Reference](./README.md)

---

# Conditionals And Switch Patterns

Use this page when you need to choose between `if`, `else if`, and `switch`.
For the lesson version, see [Conditionals and Comparison](../course/02-core-language-basics/03-conditionals-and-comparison.md).

## Basic `if`

```go
if score >= 70 {
    fmt.Println("passing")
}
```

Go conditions do not use parentheses:

```go
if score >= 70 {
    // good
}
```

Avoid:

```go
if (score >= 70) {
    // not Go style
}
```

## `if` / `else`

```go
if ready {
    fmt.Println("ready")
} else {
    fmt.Println("needs inspection")
}
```

Use this for a simple two-path decision.

## `else if`

```go
if score >= 90 {
    grade = "A"
} else if score >= 80 {
    grade = "B"
} else if score >= 70 {
    grade = "C"
} else {
    grade = "Needs practice"
}
```

Order matters. The first true branch runs.

## Guard Clauses

Guard clauses handle invalid or special cases early:

```go
func describeCapacity(used, capacity int) string {
    if capacity <= 0 {
        return "invalid capacity"
    }

    if used >= capacity {
        return "full"
    }

    return "space available"
}
```

This keeps the normal path less nested.

## `if` With Short Statement

Go can declare a variable inside an `if`:

```go
if value, ok := readiness["Van"]; ok {
    fmt.Println("status found:", value)
} else {
    fmt.Println("status missing")
}
```

`value` and `ok` only exist inside the `if` / `else` statement.

This is common with map lookups and error handling.

## Error Handling Pattern

```go
result, err := loadReport("report.json")
if err != nil {
    return err
}

fmt.Println(result)
```

Go code often checks errors immediately after a call that can fail.

## Basic `switch`

```go
switch role {
case "owner":
    access = "full"
case "staff":
    access = "write"
default:
    access = "read"
}
```

Go `switch` cases do not fall through by default. After a matching case runs,
the switch is done.

## Switch Without A Value

This is useful for range-style conditions:

```go
switch {
case score >= 90:
    grade = "A"
case score >= 80:
    grade = "B"
case score >= 70:
    grade = "C"
default:
    grade = "Needs practice"
}
```

This can read cleaner than a long `if` chain when the branches are parallel.

## No Ternary Operator

Go has no ternary operator:

```text
condition ? a : b
```

Use a clear `if`:

```go
label := "not ready"
if ready {
    label = "ready"
}
```

This is intentionally explicit.

## Risk Notices

- Do not write deeply nested `if` blocks when guard clauses would be clearer.
- Do not ignore the `ok` result from map lookup when missing keys matter.
- Do not expect `switch` cases to fall through like C unless you explicitly use
  `fallthrough`, which is rare.
- Do not force `switch` when two simple branches would be clearer.

## Related Lessons

- [Conditionals and Comparison](../course/02-core-language-basics/03-conditionals-and-comparison.md)
- [Arrays, Slices, and Maps](../course/02-core-language-basics/05-arrays-slices-and-maps-in-plain-language.md)
- [Chapter 02 Checkpoint](../course/02-core-language-basics/06-chapter-02-checkpoint.md)

---

[Reference Index](./README.md) / [Go](../README.md) / [Home](../../../README.md)
