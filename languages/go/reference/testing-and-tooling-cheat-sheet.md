<h1 align="center">
    <img width="99" alt="Go logo" src="../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../README.md) / [Go](../README.md) / [Reference](./README.md)

---

# Go Testing And Tooling Cheat Sheet

Use this page when writing or running Go tests. For the guided lessons, see
[Reading and Writing Basic Tests](../course/04-testing-and-tooling/01-reading-and-writing-basic-tests.md) and [A Beginner-Friendly Go Quality Workflow](../course/04-testing-and-tooling/05-a-beginner-friendly-go-quality-workflow.md).

## Common Workflow

```bash
gofmt -w .
go vet ./...
go test ./...
```

## Test File Rules

Test files end with:

```text
_test.go
```

Test functions start with `Test` and receive `*testing.T`:

```go
func TestAdd(t *testing.T) {
    got := 2 + 2
    want := 4

    if got != want {
        t.Fatalf("got %d, want %d", got, want)
    }
}
```

## Table-Driven Test Pattern

```go
func TestDescribeStatus(t *testing.T) {
    cases := []struct {
        name string
        used int
        capacity int
        want string
    }{
        {name: "empty", used: 0, capacity: 3, want: "empty"},
        {name: "full", used: 3, capacity: 3, want: "full"},
    }

    for _, tc := range cases {
        t.Run(tc.name, func(t *testing.T) {
            got := DescribeStatus(tc.used, tc.capacity)
            if got != tc.want {
                t.Fatalf("got %q, want %q", got, tc.want)
            }
        })
    }
}
```

## Focused Test Commands

Run current package:

```bash
go test
```

Run all packages:

```bash
go test ./...
```

Run one test:

```bash
go test -run TestDescribeStatus
```

Verbose output:

```bash
go test -v ./...
```

Race detector for concurrent code:

```bash
go test -race ./...
```

## Tooling Commands

Format:

```bash
gofmt -w .
```

Vet:

```bash
go vet ./...
```

Docs:

```bash
go doc fmt.Println
go doc ./garage
```

## Good Failure Messages

Prefer:

```go
t.Fatalf("got %d, want %d", got, want)
```

Avoid:

```go
t.Fatal("failed")
```

The future reader needs to know what differed.

## Risk Notices

- Do not forget `_test.go`.
- Do not name helper functions `TestSomething` unless they are real tests.
- Do not rely only on `go run`; tests are repeatable checks.
- Do not use sleeps to make concurrent tests pass.
- Do not hide unclear failures behind vague assertion text.

---

[Reference Index](./README.md) / [Go](../README.md) / [Home](../../../README.md)
