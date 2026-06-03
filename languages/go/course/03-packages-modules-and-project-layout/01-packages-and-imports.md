<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 03](./README.md)

---

# Packages And Imports

Packages let Go code be grouped by responsibility instead of becoming one giant
file.

Beginner translation:

```text
folder of related .go files = package
import path                  = how another package refers to it
```

## A Tiny Project

Start with this layout:

```text
garageapp/
  go.mod
  main.go
  garage/
    garage.go
```

Create the module:

```bash
go mod init example.com/garageapp
```

The module path is:

```text
example.com/garageapp
```

Inside this project, package import paths begin with that module path.

## The `main` Package

`main.go`:

```go
package main

import (
    "fmt"

    "example.com/garageapp/garage"
)

func main() {
    status := garage.Describe(2, 5)
    fmt.Println(status)
}
```

Important parts:

- `package main` means this file belongs to the executable app.
- `fmt` is a standard library package.
- `example.com/garageapp/garage` is your local package import path.
- `garage.Describe` calls an exported function from the `garage` package.

## The Reusable Package

`garage/garage.go`:

```go
package garage

import "fmt"

func Describe(cars int, capacity int) string {
    return fmt.Sprintf("%d of %d spaces used", cars, capacity)
}
```

Important parts:

- `package garage` must match the package name for files in this folder.
- `Describe` starts with a capital letter, so other packages can use it.
- `fmt.Sprintf` formats text and returns it as a string.

Run from the module root:

```bash
go run .
```

Expected output:

```text
2 of 5 spaces used
```

## Package Name Versus Folder Name

Usually, the folder name and package name match:

```text
garage/garage.go -> package garage
```

That makes imports easy to read:

```go
import "example.com/garageapp/garage"
```

Then code uses:

```go
garage.Describe(2, 5)
```

Avoid cute or vague package names. Names like `helpers`, `utils`, and `common`
often become junk drawers.

## Import Groups

Go convention separates standard library imports from your project imports:

```go
import (
    "fmt"
    "strings"

    "example.com/garageapp/garage"
)
```

`gofmt` handles spacing and ordering inside groups.

Run:

```bash
gofmt -w .
```

## Exported And Unexported Names

Go uses capitalization for visibility.

```go
func Describe(cars int, capacity int) string {
    return statusText(cars, capacity)
}

func statusText(cars int, capacity int) string {
    return fmt.Sprintf("%d of %d spaces used", cars, capacity)
}
```

`Describe` is exported because it starts with `D`.

`statusText` is unexported because it starts with `s`.

Other packages can call:

```go
garage.Describe(2, 5)
```

Other packages cannot call:

```go
garage.statusText(2, 5)
```

That is a good thing. Export only the names other packages should depend on.

## Visual Model

```text
main package
  |
  | imports
  v
garage package
  |
  | exposes exported names
  v
Describe
```

The `main` package should coordinate the app. Reusable packages should contain
focused behavior.

## Common Beginner Mistakes

### Importing The Folder Without The Module Path

This is wrong in module mode:

```go
import "garage"
```

Use the full module-based path:

```go
import "example.com/garageapp/garage"
```

### Mixing Package Names In One Folder

Do not put `package main` and `package garage` files in the same folder.

This is wrong:

```text
garageapp/
  main.go       package main
  garage.go     package garage
```

Use separate folders:

```text
garageapp/
  main.go
  garage/
    garage.go
```

### Exporting Everything

If every function starts with a capital letter, your package has a large public
surface. That makes future changes harder.

Start small:

- export functions callers truly need
- keep helper functions unexported
- add exports only when another package has a real reason to call them

## Practice

Add a second function to `garage/garage.go`:

```go
func HasSpace(cars int, capacity int) bool {
    return cars < capacity
}
```

Call it from `main.go`:

```go
if garage.HasSpace(2, 5) {
    fmt.Println("space available")
}
```

Then add an unexported helper and confirm `main` cannot call it.

---

[**Next ->** Exported and Unexported Names](./02-exported-and-unexported-names.md)  
[**<- Previous** Chapter 02](../02-core-language-basics/README.md)
