<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 01](./README.md)

---

# Testing, Formatting, and Documentation Basics

> Good Go habits start early: readable code, runnable tests, and clear names.

**You will learn:**
- How Go tests are named and run
- Why Go beginners usually do not start with an `assert` function
- Why formatting and documentation are part of normal development

**Before this page, you should know:** [Go Modules and Dependencies](./05-go-modules-and-dependencies.md)

---

## What a basic Go test looks like

A file ending in `_test.go` can hold tests.
A basic test uses the `testing` package.

```go
package main

import "testing"

func TestAdd(t *testing.T) {
    got := 2 + 2
    want := 4

    if got != want {
        t.Fatalf("got %d, want %d", got, want)
    }
}
```

## Why there is no built-in beginner `assert`

Many languages teach testing with an `assert(...)` helper.
Go's standard testing style often starts with plain comparisons and clear
failure messages instead.

That is why this pattern is common:

```go
if got != want {
    t.Fatalf("got %d, want %d", got, want)
}
```

It may look more manual than `assert`, but it is explicit and easy to read.

> [!NOTE]
> Third-party assertion libraries exist, but beginners should understand the
> plain standard-library testing style first.

## Formatting and docs

Use:

```bash
go fmt ./...
go test ./...
go doc fmt
```

- `go fmt ./...` formats your code
- `go test ./...` runs tests
- `go doc fmt` shows documentation for a package

## Expected beginner habit

When you change code:
1. format it
2. test it
3. read any failure carefully
4. fix the cause, not the symptom

---

## Recap

- Go tests usually start with the standard `testing` package.
- You do not need an assertion library to learn testing well.
- Formatting, testing, and docs lookup belong in the normal loop.

## Try it yourself

Write one small function and one test for it, then intentionally make the test
fail so you can read the failure output without panic.

---

[**Next ->** Chapter 02](../02-core-language-basics/README.md)
[**<- Previous** Go Modules and Dependencies](./05-go-modules-and-dependencies.md)


