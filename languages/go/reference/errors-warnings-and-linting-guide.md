<h1 align="center">
    <img width="99" alt="Go logo" src="../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../README.md) / [Go](../README.md) / [Reference](./README.md)

---

# Go Errors, Warnings, And Linting Guide

Use this page when `go test`, `go vet`, or a compiler message tells you
something is wrong. For the workflow lesson, see [A Beginner-Friendly Go Quality Workflow](../course/04-testing-and-tooling/05-a-beginner-friendly-go-quality-workflow.md).

## Core Command Loop

```bash
gofmt -w .
go vet ./...
go test ./...
```

Read this as:

```text
format first
inspect suspicious code second
test behavior third
```

## Compiler Error Pattern

Example:

```text
./main.go:12:15: undefined: totla
```

Read it as:

```text
file: ./main.go
line: 12
column: 15
problem: the name totla is not defined
```

Start at the file and line. Fix the first clear error, then rerun the command.

## Test Failure Pattern

Example:

```text
--- FAIL: TestAdd (0.00s)
    add_test.go:14: got 5, want 4
FAIL
```

Read in order:

```text
test name
file and line
got value
want value
```

Good tests tell you both what happened and what should have happened.

## `go vet` Warning Pattern

Example:

```text
./main.go:27: fmt.Printf format %d has arg name of wrong type string
```

Read it as:

```text
In main.go line 27, Printf expected an integer for %d but received a string.
```

Fix either the format string or the value.

## Common Fixes

Undefined name:

```go
fmt.Println(totla)
```

Usually means a typo or missing declaration.

Unused variable:

```go
value := 10
```

Use it, remove it, or replace with `_` only when intentionally ignored.

Wrong format:

```go
fmt.Printf("%d\n", name)
```

Use `%s` for strings:

```go
fmt.Printf("%s\n", name)
```

## Triage Flow

```text
1. Rerun the failing command.
2. Read the first file and line.
3. Identify the smallest likely cause.
4. Fix one thing.
5. Rerun the same command.
6. Continue until clean.
```

Do not randomly edit several files at once. Small fixes keep the feedback clear.

## Risk Notices

- Do not ignore returned errors from functions.
- Do not skip `go vet` just because tests pass.
- Do not hide unused values with `_` unless ignoring is intentional.
- Do not write vague test failures such as `failed`; include `got` and `want`.
- Do not assume a race-free result just because a concurrent test passed once.

## Related References

- [Testing and Tooling Cheat Sheet](./testing-and-tooling-cheat-sheet.md)
- [Go Commands](./go-commands.md)
- [Concurrency Cheat Sheet](./concurrency-cheat-sheet.md)

---

[Reference Index](./README.md) / [Go](../README.md) / [Home](../../../README.md)
