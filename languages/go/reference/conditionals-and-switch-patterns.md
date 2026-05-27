<h1 align="center">
    <img width="99" alt="Go logo" src="../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

<div align="center">

[Home](../../../README.md) · [Track](../README.md) · [Reference Index](./README.md)

</div>

---

# Conditionals and Switch Patterns

## `if` only

```go
if user == nil {
    return
}
```

## `if / else if / else`

```go
if score >= 90 {
    grade = "A"
} else if score >= 80 {
    grade = "B"
} else {
    grade = "C"
}
```

## `switch`

```go
switch role {
case "owner":
    access = "full"
default:
    access = "read"
}
```

## Important note

Go has no ternary operator (`condition ? a : b`).
Use explicit `if` blocks.

---

<div align="center">

[← Reference Index](./README.md) · [Track](../README.md) · [Home](../../../README.md)

</div>
