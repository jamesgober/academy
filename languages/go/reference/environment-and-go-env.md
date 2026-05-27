<h1 align="center">
    <img width="99" alt="Go logo" src="../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

<div align="center">

[Home](../../../README.md) · [Track](../README.md) · [Reference Index](./README.md)

</div>

---

# Environment and `go env`

> Quick lookup for the environment values beginners see most often in Go.

## Useful `go env` values

| Name | Meaning |
|------|---------|
| `GOPATH` | Workspace/cache path used by Go tools |
| `GOROOT` | Go toolchain install path |
| `GOMODCACHE` | Downloaded module cache |
| `GOOS` | Target operating system |
| `GOARCH` | Target CPU architecture |

## Common uses

```bash
go env GOPATH
go env GOROOT
go env GOOS GOARCH
```

## Beginner reminders

- you usually inspect these values more often than you manually change them
- environment values help explain why builds differ across machines
- do not hard-code machine-specific paths into your code

---

<div align="center">

[← Reference Index](./README.md) · [Track](../README.md) · [Home](../../../README.md)

</div>
