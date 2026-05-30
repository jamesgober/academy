<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 04](./README.md)

---

# Reading and Writing Basic Tests

> A test is code that checks whether other code behaves the way you expect.

**You will learn:**
- What a Go test file looks like
- Why tests use the `testing` package
- How to describe `got` and `want` clearly

**Before this page, you should know:** [Chapter 03](../03-packages-modules-and-project-layout/README.md)

---

## Basic test shape

```go
func TestAdd(t *testing.T) {
    got := 2 + 2
    want := 4

    if got != want {
        t.Fatalf("got %d, want %d", got, want)
    }
}
```

## Read it in English

- run a test named `TestAdd`
- calculate `got`
- compare it to `want`
- fail with a clear message if they differ

> [!IMPORTANT]
> The `got` and `want` pattern matters because it makes failure output easy to understand.

---

## Recap

- A test checks expected behavior.
- Go uses the `testing` package by default.
- Clear failure messages matter as much as the check itself.

## Try it yourself

Write a function that adds two numbers and a test that verifies one case.

---

[**Next ->** Table-Driven Tests Without Confusion](./02-table-driven-tests-without-confusion.md)
[**<- Previous** Chapter 03](../03-packages-modules-and-project-layout/README.md)


