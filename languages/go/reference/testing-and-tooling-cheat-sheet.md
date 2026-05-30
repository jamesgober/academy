<h1 align="center">
    <img width="99" alt="Go logo" src="../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../README.md) / [Go](../README.md) / [Reference](./README.md)

---

# Testing and Tooling Cheat Sheet

> Fast lookup for beginner-safe Go quality habits.

## Common workflow

```bash
go fmt ./...
go test ./...
```

## Basic test pattern

```go
func TestAdd(t *testing.T) {
    got := 2 + 2
    want := 4

    if got != want {
        t.Fatalf("got %d, want %d", got, want)
    }
}
```

## Important reminders

- test files end with `_test.go`
- tests usually use the standard `testing` package first
- beginners do not need an assertion library to write good tests
- clear failure messages matter

## Quick diagnostic prompts

- did the file name end with `_test.go`?
- did the test function start with `Test`?
- does the failure message explain both got and want?

---

<div align="center">

[Reference Index](./README.md)  /  [Go](../README.md)  /  [Home](../../../README.md)

</div>



