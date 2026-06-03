<h1 align="center">
    <img width="99" alt="Go logo" src="../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../README.md) / [Go](../README.md) / [Reference](./README.md)

---

# Go Environment And `go env`

Use this page when Go paths, module cache, operating-system targets, or toolchain
settings are confusing.

## Inspect Values

```bash
go env
go env GOPATH
go env GOROOT
go env GOOS GOARCH
```

Most beginners inspect these values more often than they change them.

## Common Values

| Name | Meaning |
|---|---|
| `GOROOT` | Where the Go toolchain is installed. |
| `GOPATH` | Workspace/cache area used by Go tools. |
| `GOMOD` | Path to the active `go.mod`, or empty/special value outside a module. |
| `GOMODCACHE` | Downloaded module cache. |
| `GOCACHE` | Build cache. |
| `GOOS` | Target operating system. |
| `GOARCH` | Target CPU architecture. |
| `GOVERSION` | Current Go toolchain version. |

## Module Root Check

From inside a module:

```bash
go env GOMOD
```

You should see a path ending in:

```text
go.mod
```

If Go cannot find your module, move to the folder containing `go.mod`.

## Cross-Compile Preview

Go can target another operating system:

```bash
GOOS=linux GOARCH=amd64 go build
```

PowerShell:

```powershell
$env:GOOS='linux'
$env:GOARCH='amd64'
go build
Remove-Item Env:GOOS
Remove-Item Env:GOARCH
```

This is useful later for deployment. Beginners should first get normal local
builds working.

## Cache Notes

Downloaded dependencies live in `GOMODCACHE`.

Build artifacts live in `GOCACHE`.

Usually you do not edit these folders manually. If cache behavior seems broken,
prefer Go commands over hand deletion:

```bash
go clean -cache
go clean -modcache
```

`go clean -modcache` removes downloaded modules, so the next build may need to
download them again.

## Risk Notices

- Do not hard-code machine-specific paths into Go code.
- Do not manually edit files in the module cache.
- Do not change `GOROOT` casually.
- Do not assume another developer has the same `GOPATH`.
- Be careful leaving `GOOS` or `GOARCH` set in your shell after cross-compiling.

## Related References

- [Go Commands](./go-commands.md)
- [Packages, Modules, and Layout](./packages-modules-and-layout.md)

---

[Reference Index](./README.md) / [Go](../README.md) / [Home](../../../README.md)
