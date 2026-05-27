<h1 align="center">
    <img width="99" alt="Go logo" src="../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

<div align="center">

[Home](../../../README.md) · [Track](../README.md) · [Reference Index](./README.md)

</div>

---

# Functions, Parameters, and Returns Cheat Sheet

## Basic form

```go
func greet(name string) string {
    return "hi " + name
}
```

## Multiple returns

```go
func divide(a, b int) (int, int) {
    return a / b, a % b
}
```

## Variadic parameter

```go
func sum(values ...int) int
```

## Common production signature

```go
func parse(raw string) (Result, error)
```

## Design prompts

- Is this function doing one clear job?
- Are parameter names specific and readable?
- Is `(value, error)` clearer than panic for this case?

---

<div align="center">

[← Reference Index](./README.md) · [Track](../README.md) · [Home](../../../README.md)

</div>
