<h1 align="center">
    <img width="99" alt="Go logo" src="../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../README.md) / [Go](../README.md) / [Reference](./README.md)

---

# Errors, Warnings, and Linting Guide

## Test error output pattern

```text
--- FAIL: TestAdd (0.00s)
    add_test.go:14: got 5, want 4
```

Read in order:
- test name
- file and line
- mismatch details

## Vet/lint warning output pattern

```text
./main.go:27: printf format %d has arg name of wrong type string
```

Read in order:
- file and line
- operation
- expected type versus actual type

## Fast triage flow

1. Reproduce issue.
2. Jump to file/line in message.
3. Confirm root cause in smallest function.
4. Fix code.
5. Rerun checks.

## Command loop

```bash
go fmt ./...
go vet ./...
go test ./...
```

---

<div align="center">

[Reference Index](./README.md)  /  [Go](../README.md)  /  [Home](../../../README.md)

</div>



