<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 04](./README.md)

---

# A Beginner-Friendly Go Quality Workflow

A quality workflow is a repeatable order for staying out of trouble.

It is not a ceremony. It is a short checklist you run before you trust your code.

## The Core Loop

Run these from the module root:

```bash
gofmt -w .
go vet ./...
go test ./...
```

Read them as:

```text
format the code
look for suspicious patterns
run the tests
```

## Why This Order Works

Format first:

```bash
gofmt -w .
```

Formatting removes style noise. Go teams should not argue about indentation
because `gofmt` decides it.

Vet second:

```bash
go vet ./...
```

`go vet` looks for code that compiles but is suspicious, such as incorrect
format strings.

Test third:

```bash
go test ./...
```

Tests check behavior after the code is formatted and structurally sane.

## A Practical Development Rhythm

Use this loop while building:

```text
1. Write a small change.
2. Run gofmt.
3. Run the focused test if one exists.
4. Fix failures.
5. Run go vet ./...
6. Run go test ./...
```

When you are learning, small changes are easier to understand than large mystery
changes.

## Focused Tests While Working

Run all tests:

```bash
go test ./...
```

Run tests in the current package:

```bash
go test
```

Run one named test:

```bash
go test -run TestDescribeStatus
```

Run verbose output:

```bash
go test -v ./...
```

Focused tests are useful while editing. Full tests are useful before calling the
work done.

## What Clean Looks Like

A clean `go test ./...` might look like:

```text
ok   example.com/garage/status  0.003s
ok   example.com/garage/report  0.004s
```

A package with no test files might show:

```text
?    example.com/garage/cmd/garage  [no test files]
```

That does not mean failure. It means that package had no `_test.go` files.

## What To Do When Something Fails

Do not randomly edit.

Use this triage order:

```text
1. Read the command that failed.
2. Read the first file and line.
3. Read the message.
4. Fix one cause.
5. Rerun the same command.
```

If `gofmt` changes files, review the diff.

If `go vet` fails, treat it seriously.

If tests fail, read the `got` and `want` values before changing code.

## Optional: Race Check For Concurrent Code

When you write goroutines that share data, also run:

```bash
go test -race ./...
```

The race detector is slower, but it can catch dangerous shared-memory bugs.

Use it especially for Chapter 05 concurrency work.

## Pre-Commit Checklist

Before you commit or submit work:

```text
gofmt -w .
go vet ./...
go test ./...
```

For concurrent code:

```text
go test -race ./...
```

Then ask:

- Did I add or update tests for behavior I changed?
- Did I remove debug prints?
- Are exported names intentional?
- Can another beginner read this package tomorrow?

## Common Beginner Mistakes

### Running Commands Outside The Module

If Go cannot find your module, check for `go.mod`:

```bash
ls
```

Run quality commands from the folder that contains `go.mod`.

### Only Running The Program

`go run .` is useful, but it is not a quality workflow. It only proves one path
started.

Use tests for repeatable behavior checks.

### Ignoring Generated Formatting Changes

If `gofmt` changes files, that is expected. Review the changes so you learn the
style Go expects.

## Practice

In a practice module:

1. Run `gofmt -w .`.
2. Run `go vet ./...`.
3. Run `go test ./...`.
4. Introduce one test failure.
5. Run `go test ./...` again.
6. Read the failure and fix it.

The goal is to make the workflow boring. Boring quality checks are good quality
checks.

---

[**Next ->** Chapter 04 Capstone](./06-chapter-04-capstone.md)  
[**<- Previous** Formatting, Vetting, and Documentation Tools](./04-formatting-vetting-and-documentation-tools.md)
