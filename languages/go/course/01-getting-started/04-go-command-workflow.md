<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 01](./README.md)

---

# Go Command Workflow

> Learn the small command loop you will use every day.

**You will learn:**
- What `go run`, `go test`, and `go fmt` do
- How to build a repeatable beginner workflow
- Why formatting and testing belong early

**Before this page, you should know:** [How Go Files, Packages, and `main` Work](./03-how-go-files-packages-and-main-work.md)

---

## Daily command loop

Use this order while learning:

1. `go fmt ./...`
2. `go test ./...`
3. `go run .`

Why this order:
- format first so the code stays readable
- test second so mistakes are caught before manual clicking around
- run last to see the actual result

## What each command does

- `go fmt ./...` formats packages under the current module
- `go test ./...` runs tests in all packages under the current module
- `go run .` runs the current package if it is runnable

> [!IMPORTANT]
> Go has a strong formatting culture. Do not fight the formatter. Let the tool win.

## A note about `./...`

`./...` means "this folder and all nested packages below it."
That pattern appears often in Go commands.

## When tests do not exist yet

If you have not written tests yet, `go test ./...` may still succeed.
That is normal. Later chapters will teach how to add real tests.

---

## Recap

- Use a simple workflow: format, test, run.
- `./...` means current directory and subpackages.
- Go tools are designed to be used often, not only at the end.

## Try it yourself

Run `go fmt ./...` in your practice folder even if you think the file already
looks clean. Then run `go run .` again.

---

[**Next ->** Go Modules and Dependencies](./05-go-modules-and-dependencies.md)
[**<- Previous** How Go Files, Packages, and `main` Work](./03-how-go-files-packages-and-main-work.md)


