<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 04](./README.md)

---

# Table-Driven Tests Without Confusion

> A table-driven test uses one test body with many test cases. It is one of the
> most common Go testing patterns because it makes edge cases easy to see.

**You will learn:**
- What a test case table is
- Why Go uses slices of structs for test cases
- How `t.Run` names subtests
- How to add edge cases
- When table tests become too much

**Before this page, you should know:** [Reading And Writing Basic Tests](./01-reading-and-writing-basic-tests.md)

---

## Start With Repeated Tests

This works:

```go
func TestIsPassing75(t *testing.T) {
    got := IsPassing(75)
    want := true

    if got != want {
        t.Fatalf("IsPassing(75) = %v, want %v", got, want)
    }
}

func TestIsPassing40(t *testing.T) {
    got := IsPassing(40)
    want := false

    if got != want {
        t.Fatalf("IsPassing(40) = %v, want %v", got, want)
    }
}
```

But the structure repeats.

---

## Table-Driven Version

```go
func TestIsPassing(t *testing.T) {
    tests := []struct {
        name  string
        score int
        want  bool
    }{
        {name: "passing score", score: 75, want: true},
        {name: "failing score", score: 40, want: false},
        {name: "boundary score", score: 60, want: true},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got := IsPassing(tt.score)

            if got != tt.want {
                t.Fatalf("IsPassing(%d) = %v, want %v",
                    tt.score,
                    got,
                    tt.want,
                )
            }
        })
    }
}
```

Read:

```text
Create test cases.
Loop over cases.
Run each case as a named subtest.
Compare got and want.
```

---

## Why `t.Run` Matters

If one case fails, output includes the subtest name.

Example:

```text
--- FAIL: TestIsPassing/boundary_score
```

That points you to the exact case.

Good test names describe why the case exists:

- `"passing score"`
- `"failing score"`
- `"boundary score"`
- `"negative score"`
- `"empty input"`

---

## Test Case Struct

This is an anonymous struct type:

```go
tests := []struct {
    name  string
    score int
    want  bool
}{}
```

Plain language:

```text
tests is a slice.
Each item has name, score, and want fields.
```

This is common in Go because it keeps test data close to the test.

---

## Edge Cases

Add cases for:

- zero
- empty string
- nil slice
- minimum allowed value
- maximum allowed value
- invalid input
- duplicate input
- boundary between two behaviors

Example:

```go
{name: "exactly passing", score: 60, want: true},
{name: "one below passing", score: 59, want: false},
```

Boundary tests catch many real bugs.

---

## When Not To Use A Table

Table-driven tests are great when cases share the same structure.

Do not force unrelated behavior into one huge table.

If setup or assertions become hard to read, split the tests.

Rule:

```text
The table should make the test easier to scan, not more clever.
```

---

## Real Example: Shipping Cost

```go
package shipping

func Cost(weight int) int {
    switch {
    case weight <= 0:
        return 0
    case weight <= 10:
        return 5
    case weight <= 50:
        return 10
    default:
        return 25
    }
}
```

Test:

```go
package shipping

import "testing"

func TestCost(t *testing.T) {
    tests := []struct {
        name   string
        weight int
        want   int
    }{
        {name: "invalid weight", weight: 0, want: 0},
        {name: "small package", weight: 10, want: 5},
        {name: "medium package", weight: 50, want: 10},
        {name: "large package", weight: 51, want: 25},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got := Cost(tt.weight)

            if got != tt.want {
                t.Fatalf("Cost(%d) = %d, want %d",
                    tt.weight,
                    got,
                    tt.want,
                )
            }
        })
    }
}
```

---

## Chapter Checkpoint

You should now be able to answer:

- What is a table-driven test?
- Why does Go use slices of structs for test cases?
- What does `t.Run` do?
- Why are boundary cases important?
- When should you split a table into separate tests?

---

## Recap

- Table tests reuse one test structure for many cases.
- Test cases are often stored in a slice of structs.
- `t.Run` gives each case a name.
- Edge cases belong in the table.
- Tables should improve readability.

## Try It Yourself

Convert your `Multiply` test into a table with:

- positive numbers
- zero
- negative number
- one boundary case you choose

---

[**Next ->** Reading Test Failures And Debugging Faster](./03-reading-test-failures-and-debugging-faster.md)  
[**<- Previous** Reading And Writing Basic Tests](./01-reading-and-writing-basic-tests.md)
