<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 04](./README.md)

---

# Chapter 04 Capstone: Tested Status Package

> This capstone proves you can build a small Go package, write useful tests,
> read failures, and run the built-in quality workflow.

**You will build:**
- a status package
- exported functions
- table-driven tests
- useful failure messages
- formatting/vetting/testing workflow
- a short quality note

---

## Final Project Shape

```text
status-demo/
  go.mod
  status/
    status.go
    status_test.go
  QUALITY.md
```

Create:

```bash
mkdir status-demo
cd status-demo
go mod init example.com/status-demo
mkdir status
```

---

## Step 1: Package Code

`status/status.go`:

```go
// Package status provides simple score and inventory status helpers.
package status

func Grade(score int) string {
    switch {
    case score < 0:
        return "invalid"
    case score >= 90:
        return "A"
    case score >= 80:
        return "B"
    case score >= 70:
        return "C"
    case score >= 60:
        return "D"
    default:
        return "F"
    }
}

func StockLabel(quantity int) string {
    switch {
    case quantity < 0:
        return "invalid"
    case quantity == 0:
        return "out"
    case quantity <= 5:
        return "low"
    default:
        return "ok"
    }
}
```

Why these functions?

They are small, rule-based, and easy to test with boundary cases.

---

## Step 2: Grade Tests

`status/status_test.go`:

```go
package status

import "testing"

func TestGrade(t *testing.T) {
    tests := []struct {
        name  string
        score int
        want  string
    }{
        {name: "invalid negative", score: -1, want: "invalid"},
        {name: "A boundary", score: 90, want: "A"},
        {name: "B boundary", score: 80, want: "B"},
        {name: "C boundary", score: 70, want: "C"},
        {name: "D boundary", score: 60, want: "D"},
        {name: "failing score", score: 59, want: "F"},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got := Grade(tt.score)

            if got != tt.want {
                t.Fatalf("Grade(%d) = %q, want %q",
                    tt.score,
                    got,
                    tt.want,
                )
            }
        })
    }
}
```

Run:

```bash
go test ./...
```

---

## Step 3: Stock Tests

Add:

```go
func TestStockLabel(t *testing.T) {
    tests := []struct {
        name     string
        quantity int
        want     string
    }{
        {name: "invalid negative", quantity: -1, want: "invalid"},
        {name: "out of stock", quantity: 0, want: "out"},
        {name: "low boundary", quantity: 5, want: "low"},
        {name: "ok boundary", quantity: 6, want: "ok"},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got := StockLabel(tt.quantity)

            if got != tt.want {
                t.Fatalf("StockLabel(%d) = %q, want %q",
                    tt.quantity,
                    got,
                    tt.want,
                )
            }
        })
    }
}
```

---

## Step 4: Quality Workflow

Run:

```bash
gofmt -w .
go vet ./...
go test ./...
go mod tidy
```

Expected:

```text
formatting applied
vet clean
tests pass
module files tidy
```

---

## Step 5: Failure Practice

Intentionally break:

```go
case score >= 90:
    return "B"
```

Run:

```bash
go test ./...
```

Read the failure:

```text
Which test failed?
Which subtest failed?
What was got?
What was want?
What input caused it?
```

Then fix the code and rerun all tests.

---

## Step 6: Quality Note

`QUALITY.md`:

```md
# Status Demo Quality Workflow

## Commands

gofmt -w .
go vet ./...
go test ./...
go mod tidy

## Test Coverage

Grade tests cover invalid input and grade boundaries.
StockLabel tests cover invalid input, out of stock, low stock, and ok stock.

## Failure Message Style

Failures include function name, input, got value, and want value.
```

---

## Completion Checklist

- package has useful exported functions
- tests use `got` and `want`
- tests include boundary cases
- table-driven tests use `t.Run`
- failure messages include input
- `gofmt -w .` run
- `go vet ./...` clean
- `go test ./...` passing
- `go mod tidy` run
- `QUALITY.md` explains workflow

---

## Recap

You now have the core Go testing workflow:

- write small package behavior
- test with table-driven cases
- read failures from input/got/want
- format with `gofmt`
- vet with `go vet`
- test all packages with `go test ./...`
- tidy module files with `go mod tidy`

That is a real Go development loop.

---

[**Next ->** Chapter 05](../05-concurrency-and-practical-applications/README.md)  
[**<- Previous** A Beginner-Friendly Go Quality Workflow](./05-a-beginner-friendly-go-quality-workflow.md)
