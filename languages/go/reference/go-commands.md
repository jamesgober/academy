<h1 align="center">
    <img width="99" alt="Go logo" src="../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

<div align="center">

[Home](../../../README.md) · [Track](../README.md) · [Reference Index](./README.md)

</div>

---

# Go Commands Reference

> Quick lookup for the Go commands you will use most often.

## At a glance

| Command | Purpose | Typical use |
|---------|---------|-------------|
| `go run .` | Run current package | Quick local execution |
| `go test ./...` | Run tests in all packages | Validate behavior |
| `go fmt ./...` | Format code | Keep style consistent |
| `go mod init <name>` | Start a module | Begin a project |
| `go mod tidy` | Clean dependency files | After dependency changes |
| `go doc <pkg>` | Read package docs | Lookup while learning |
| `go env` | Show Go environment values | Inspect toolchain config |

## Common command forms

```bash
go run .
go fmt ./...
go test ./...
go mod init example/project
go mod tidy
go doc fmt
go env GOPATH
```

> [!TIP]
> Prefer `go test ./...` over testing one package by hand once your project has
> multiple folders.

---

<div align="center">

[← Reference Index](./README.md) · [Track](../README.md) · [Home](../../../README.md)

</div>
