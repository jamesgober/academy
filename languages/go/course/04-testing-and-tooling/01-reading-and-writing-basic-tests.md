<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 04](./README.md)

---

# Reading And Writing Basic Tests

> A Go test is a normal Go function that checks behavior. Tests are how you prove
> your package still works after you change it.

**You will learn:**
- Where Go test files live
- How `testing.T` works
- How to use `got` and `want`
- How to run tests
- How to write useful failure messages
- How to test exported package behavior

**Before this page, you should know:** [Chapter 03](../03-packages-modules-and-project-layout/README.md)

---

## Project Shape

Create:

```text
calc-demo/
  go.mod
  calc/
    calc.go
    calc_test.go
```

Commands:

```bash
mkdir calc-demo
cd calc-demo
go mod init example.com/calc-demo
mkdir calc
```

---

## Code Under Test

`calc/calc.go`:

```go
package calc

func Add(left int, right int) int {
    return left + right
}

func IsPassing(score int) bool {
    return score >= 60
}
```

This package has two exported functions:

- `Add`
- `IsPassing`

Exported means other packages can use them because their names start with
uppercase letters.

---

## Basic Test

`calc/calc_test.go`:

```go
package calc

import "testing"

func TestAdd(t *testing.T) {
    got := Add(2, 3)
    want := 5

    if got != want {
        t.Fatalf("Add(2, 3) = %d, want %d", got, want)
    }
}
```

Run:

```bash
go test ./...
```

Expected:

```text
ok      example.com/calc-demo/calc
```

---

## Test Function Rules

Go test functions:

- live in files ending with `_test.go`
- start with `Test`
- take `t *testing.T`
- do not return a value

Shape:

```go
func TestSomething(t *testing.T) {
}
```

---

## `got` And `want`

This pattern is everywhere in Go tests:

```go
got := Add(2, 3)
want := 5
```

Read:

```text
got is what the code actually returned.
want is what the test expected.
```

Failure message:

```go
t.Fatalf("Add(2, 3) = %d, want %d", got, want)
```

Good failure messages include:

- function or behavior tested
- important input
- got value
- want value

---

## `t.Fatal` Versus `t.Errorf`

`t.Fatalf` fails and stops the current test immediately.

```go
t.Fatalf("got %d, want %d", got, want)
```

`t.Errorf` reports failure but lets the test continue.

```go
t.Errorf("got %d, want %d", got, want)
```

Beginner rule:

```text
Use Fatalf when continuing would not make sense.
Use Errorf when checking several independent things in one test.
```

---

## Test Another Function

```go
func TestIsPassing(t *testing.T) {
    got := IsPassing(75)
    want := true

    if got != want {
        t.Fatalf("IsPassing(75) = %v, want %v", got, want)
    }
}
```

Run:

```bash
go test ./...
```

---

## Package Name In Tests

Most beginner tests use the same package:

```go
package calc
```

That lets tests access unexported helpers too.

You may also see external tests:

```go
package calc_test
```

Those tests use the package like an outside caller would.

Beginner rule:

```text
Start with same-package tests while learning.
Use external package tests when you specifically want to test public API only.
```

---

## Common Mistakes

### Mistake 1: Test File Does Not End In `_test.go`

Go will not run it as a test file.

### Mistake 2: Function Does Not Start With `Test`

```go
func CheckAdd(t *testing.T) {
}
```

Go will not run it as a test.

### Mistake 3: Vague Failure Message

Weak:

```go
t.Fatal("bad")
```

Better:

```go
t.Fatalf("Add(2, 3) = %d, want %d", got, want)
```

---

## Chapter Checkpoint

You should now be able to answer:

- What file suffix do Go tests use?
- What does `t *testing.T` do?
- What does `got` mean?
- What does `want` mean?
- How do you run tests in every package?
- What makes a failure message useful?

---

## Recap

- Go tests live in `_test.go` files.
- Test functions start with `Test`.
- `go test ./...` runs tests in all packages.
- `got` and `want` make failures easy to read.
- Clear failure messages save debugging time.

## Try It Yourself

Add a `Multiply(left, right int) int` function and write a test for:

```text
Multiply(4, 5) = 20
```

---

[**Next ->** Table-Driven Tests Without Confusion](./02-table-driven-tests-without-confusion.md)  
[**<- Previous** Chapter 03](../03-packages-modules-and-project-layout/README.md)
