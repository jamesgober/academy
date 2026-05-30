<h1 align="center">
    <img width="99" alt="Go logo" src="../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../README.md) / [Go](../README.md) / [Reference](./README.md)

---

# Go Commands Reference

> Quick lookup for the Go commands you will use most often while building,
> testing, formatting, and managing modules.

## Related Lessons

- [Go Command Workflow](../course/01-getting-started/04-go-command-workflow.md)
- [Go Modules And Dependencies](../course/01-getting-started/05-go-modules-and-dependencies.md)
- [Testing, Formatting, And Documentation Basics](../course/01-getting-started/06-testing-formatting-and-documentation-basics.md)
- [A Beginner-Friendly Go Quality Workflow](../course/04-testing-and-tooling/05-a-beginner-friendly-go-quality-workflow.md)

---

## Daily Commands

| Command | Purpose | Typical use |
|---|---|---|
| `go run .` | run current package | quick local execution |
| `go test ./...` | test all packages | quality gate |
| `gofmt -w .` | format files in place | before commit |
| `go mod init <path>` | create module | new project |
| `go mod tidy` | clean dependencies | after import changes |
| `go get <module>` | add/update dependency | when adding a library |
| `go doc <pkg>` | read docs | quick lookup |
| `go env` | show Go environment | diagnose toolchain |
| `go version` | show Go version | confirm install |

---

## Common Workflow

```bash
gofmt -w .
go test ./...
go run .
go mod tidy
```

Use this after meaningful changes.

---

## Module Commands

Start:

```bash
go mod init example.com/my-app
```

Add dependency:

```bash
go get github.com/google/uuid
```

Clean:

```bash
go mod tidy
```

Notice:

```text
Commit both go.mod and go.sum when dependencies change.
```

---

## Test Commands

All packages:

```bash
go test ./...
```

One package:

```bash
go test ./garage
```

Verbose:

```bash
go test -v ./...
```

Run matching tests:

```bash
go test ./... -run TestStatus
```

Benchmark:

```bash
go test -bench=. ./...
```

---

## Documentation Commands

```bash
go doc fmt
go doc strings.Builder
go doc example.com/garage-app/garage
```

Use docs before adding dependencies. The standard library may already have what
you need.

---

## Risk Notices

- `go get` changes dependency files. Review `go.mod` and `go.sum`.
- `go test` on one package can miss failures in other packages; use `./...`.
- `go fmt ./...` prints formatted files but `gofmt -w .` writes changes.
- If imports look correct but fail, check `go.mod` module path.

---

[Reference Index](./README.md) / [Go](../README.md) / [Home](../../../README.md)
