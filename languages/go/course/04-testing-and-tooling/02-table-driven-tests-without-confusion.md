<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 04](./README.md)

---

# Table-Driven Tests Without Confusion

> Table-driven tests let one test structure cover many cases cleanly.

**You will learn:**
- What a test case table is
- Why Go uses slices of test cases so often
- How to loop through test cases readably

**Before this page, you should know:** [Reading and Writing Basic Tests](./01-reading-and-writing-basic-tests.md)

---

## Example pattern

```go
tests := []struct {
    name string
    in   int
    want int
}{
    {name: "small", in: 2, want: 4},
    {name: "zero", in: 0, want: 0},
}
```

Then loop through the cases and compare `got` to `want`.

## Why this helps

It keeps related test cases together.
It reduces repeated test code.
It makes edge cases easier to add.

> [!TIP]
> If the test table becomes hard to scan, your cases may be covering too many unrelated behaviors.

---

## Recap

- Table-driven tests group similar cases.
- A slice of test cases is the common Go pattern.
- The goal is clearer coverage, not cleverness.

## Try it yourself

Convert a one-case test into a two-case table-driven test.

---

[**Next ->** Reading Test Failures and Debugging Faster](./03-reading-test-failures-and-debugging-faster.md)
[**<- Previous** Reading and Writing Basic Tests](./01-reading-and-writing-basic-tests.md)


