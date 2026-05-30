<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 01](./README.md)

---

# Your First Go Program

> Start with one tiny program you can run end to end.

**You will learn:**
- How to create a Go project folder
- What `package main` means
- How `func main()` becomes the program entry point

**Before this page, you should know:** [Installing Go](./01-installing-go.md)

---

## Make a project folder

Create a folder for your first Go program, then open a terminal there.

```bash
mkdir hello-go
cd hello-go
```

Inside that folder, create a file named `main.go` with this code:

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Go")
}
```

## Read the file slowly

- `package main` means this file belongs to the runnable application package.
- `import "fmt"` brings in formatting and printing tools from the standard library.
- `func main()` is the function Go starts with when your program runs.
- `fmt.Println(...)` prints text and then moves to a new line.

> [!NOTE]
> A lot of beginner confusion comes from rushing past `package` and `main`.
> Slow down here. These two ideas decide whether your code is a runnable program
> or reusable library code.

## Run it

```bash
go run .
```

Expected output:

```text
Hello, Go
```

Why `.`?
It means "run the Go package in the current folder."
That becomes a useful habit once your folder has more than one file.

## Common beginner mistakes

- file is named something other than `main.go`
- code is missing `package main`
- `main` is spelled incorrectly
- terminal is not opened in the project folder

---

## Recap

- A runnable Go app starts in `package main`.
- `func main()` is the entry point.
- `go run .` runs the current package.

## Try it yourself

Change the text to print your name and one thing you want to build with Go.

---

[**Next ->** How Go Files, Packages, and `main` Work](./03-how-go-files-packages-and-main-work.md)
[**<- Previous** Installing Go](./01-installing-go.md)


