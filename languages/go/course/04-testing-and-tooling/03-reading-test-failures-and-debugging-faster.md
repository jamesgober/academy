<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Go](../../README.md) · [Chapter 04](./README.md)

</div>

---

# Reading Test Failures and Debugging Faster

> A failed test is not an insult. It is a clue.

**You will learn:**
- How to read failure output calmly
- How to separate symptom from cause
- How to debug without random guessing

**Before this page, you should know:** [Table-Driven Tests Without Confusion](./02-table-driven-tests-without-confusion.md)

---

## Start with the message

A good failure message tells you:
- what input was used
- what you got
- what you expected

Example failure output:

```text
--- FAIL: TestAdd (0.00s)
    add_test.go:14: got 5, want 4
FAIL
```

How to read it:
- `TestAdd` is the failing test
- `add_test.go:14` is the exact location
- `got 5, want 4` describes the mismatch to investigate

## Warnings and static-analysis findings

Warnings from tools like `go vet` are not test failures, but still important.
Treat them as quality issues that can become bugs later.

Example vet output pattern:

```text
./main.go:27: printf format %d has arg name of wrong type string
```

This means:
- file and line: `main.go:27`
- function call: `printf` formatting
- mismatch: `%d` expects integer but got string

## Debugging order

1. Reproduce the failure.
2. Read the failing message fully.
3. Check the smallest function involved.
4. Fix the cause.
5. Rerun the test.

> [!WARNING]
> Adding print statements everywhere before understanding the failure usually slows you down.

---

## Recap

- A failing test narrows the search area.
- Good messages speed up debugging.
- Fix causes, not only visible symptoms.

## Try it yourself

Make a small test fail on purpose, then explain the failure message in plain language.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Table-Driven Tests Without Confusion](./02-table-driven-tests-without-confusion.md) | [Chapter 04](./README.md) · [Go](../../README.md) · [Home](../../../../README.md) | [Formatting, Vetting, and Documentation Tools →](./04-formatting-vetting-and-documentation-tools.md) |

</div>
