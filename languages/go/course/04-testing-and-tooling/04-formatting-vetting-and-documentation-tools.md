<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 04](./README.md)

---

# Formatting, Vetting, And Documentation Tools

> Go's built-in tools are part of the language culture. Formatting, vetting, and
> docs are not extra ceremony; they are how Go keeps projects readable.

**You will learn:**
- What `gofmt` does
- What `go fmt` does
- What `go vet` checks
- How `go doc` helps you learn
- How these tools fit into a daily workflow

**Before this page, you should know:** [Reading Test Failures And Debugging Faster](./03-reading-test-failures-and-debugging-faster.md)

---

## Formatting

Format files in place:

```bash
gofmt -w .
```

Format packages:

```bash
go fmt ./...
```

Beginner rule:

```text
Run formatting before judging code style.
Go formatting is intentionally standardized.
```

---

## Why Formatting Matters

Go teams do not usually debate brace style or indentation.

The formatter decides.

That means code review can focus on:

- correctness
- names
- package boundaries
- tests
- error handling

---

## Vetting

Run:

```bash
go vet ./...
```

`go vet` looks for suspicious code that compiles but may be wrong.

Examples:

- wrong `fmt.Printf` format
- unreachable code
- copied locks
- suspicious struct tags
- mistakes around tests or build constraints

It is not a full linter, but it is part of the standard toolchain.

---

## Vet Example

Bug:

```go
name := "Ada"
fmt.Printf("id=%d\n", name)
```

Vet-style message:

```text
fmt.Printf format %d has arg name of wrong type string
```

Fix:

```go
fmt.Printf("name=%s\n", name)
```

---

## Documentation Lookup

Use:

```bash
go doc fmt
go doc fmt.Println
go doc strings.Builder
```

This keeps you in the terminal while learning APIs.

Example:

```bash
go doc strings.Contains
```

Then use:

```go
strings.Contains("hello", "ell")
```

---

## Package Comments

Exported packages and names should have useful comments in real projects.

```go
// Package garage provides vehicle status rules.
package garage
```

Exported function:

```go
// Status reports whether a vehicle speed is ok, warning, or invalid.
func Status(v Vehicle) string {
    // ...
}
```

Go doc comments usually start with the name being documented.

---

## Local Tool Loop

```bash
gofmt -w .
go vet ./...
go test ./...
```

Then run the app:

```bash
go run .
```

For dependency changes:

```bash
go mod tidy
```

---

## External Linters

Many teams add tools such as `golangci-lint`.

Do not start there as a beginner.

First learn:

- `gofmt`
- `go vet`
- `go test`
- `go doc`
- `go mod tidy`

Then external linting will make more sense.

---

## Common Mistakes

### Mistake 1: Hand-Formatting Forever

Let `gofmt` do it.

### Mistake 2: Treating Vet As Optional Noise

Vet findings often point to real bugs.

### Mistake 3: Googling Before Reading Local Docs

Use `go doc` for standard library lookups. It is fast and version-aware.

---

## Chapter Checkpoint

You should now be able to answer:

- What does `gofmt -w .` do?
- What does `go fmt ./...` do?
- What does `go vet ./...` check?
- What does `go doc fmt.Println` show?
- Why do exported comments matter?
- When should you run `go mod tidy`?

---

## Recap

- Formatting is automatic in Go.
- Vetting catches suspicious code patterns.
- `go doc` helps you learn APIs quickly.
- Exported comments improve package documentation.
- The standard toolchain should be your first quality workflow.

## Try It Yourself

Run these in a practice module:

```bash
gofmt -w .
go vet ./...
go test ./...
go doc fmt.Println
```

Write one sentence explaining each command.

---

[**Next ->** A Beginner-Friendly Go Quality Workflow](./05-a-beginner-friendly-go-quality-workflow.md)  
[**<- Previous** Reading Test Failures And Debugging Faster](./03-reading-test-failures-and-debugging-faster.md)
