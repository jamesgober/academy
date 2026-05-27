<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Go](../../README.md) · [Chapter 04](./README.md)

</div>

---

# Formatting, Vetting, and Documentation Tools

> Go keeps the toolchain small, but the core tools matter a lot.

**You will learn:**
- What `go fmt` does
- What `go vet` checks
- How `go doc` helps you learn packages faster

**Before this page, you should know:** [Reading Test Failures and Debugging Faster](./03-reading-test-failures-and-debugging-faster.md)

---

## Three important tools

- `go fmt ./...` formats code
- `go vet ./...` checks for suspicious patterns
- `go doc <package>` reads package documentation

## What "linting" means here

Linting is automated static checking for code issues, style risks, and bug-prone
patterns before runtime.

In the base Go toolchain:
- `go fmt` handles consistent formatting
- `go vet` handles suspicious code patterns

Many teams also add external linters for stricter policy checks.

## Why they matter

- formatting reduces style noise
- vet can catch mistakes that compile but still look wrong
- docs help you learn APIs without leaving the terminal

Example vet-style finding:

```text
./handler.go:42: unreachable code
```

Use this pattern to resolve findings:
1. Open file and line from the message.
2. Confirm what control path made code unreachable.
3. Remove or restructure code path.
4. Rerun `go vet ./...`.

> [!TIP]
> Treat `go fmt` as automatic, not optional.

---

## Recap

- Formatting keeps code consistent.
- Vetting catches suspicious code patterns.
- Documentation lookup shortens feedback loops.

## Try it yourself

Run `go doc fmt` and identify one function you recognize from earlier lessons.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Reading Test Failures and Debugging Faster](./03-reading-test-failures-and-debugging-faster.md) | [Chapter 04](./README.md) · [Go](../../README.md) · [Home](../../../../README.md) | [A Beginner-Friendly Go Quality Workflow →](./05-a-beginner-friendly-go-quality-workflow.md) |

</div>
