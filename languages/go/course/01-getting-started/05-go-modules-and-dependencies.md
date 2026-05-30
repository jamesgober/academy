<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 01](./README.md)

---

# Go Modules And Dependencies

> A Go module is your project's identity, dependency boundary, and versioned
> unit. If packages are rooms in a building, the module is the building address.

**You will learn:**
- What a module is
- What `go.mod` contains
- What `go.sum` is for
- How imports connect packages
- How dependencies are added
- How `go mod tidy` keeps files honest
- How to avoid beginner dependency mess

**Before this page, you should know:** [Go Command Workflow](./04-go-command-workflow.md)

---

## The Beginner Mental Model

Go organizes code in layers:

```text
module
  packages
    files
      functions, types, variables
```

Example:

```text
module example.com/garage
  package main
    main.go
  package garage
    garage/status.go
```

The module path is the import root.

If your module is:

```text
example.com/garage
```

then a package in folder `garage` is imported as:

```go
import "example.com/garage/garage"
```

---

## Start A Module

Create a folder:

```bash
mkdir garage-app
cd garage-app
```

Initialize:

```bash
go mod init example.com/garage-app
```

This creates `go.mod`:

```go
module example.com/garage-app

go 1.22
```

Your Go version line may differ.

Read:

```text
module example.com/garage-app
```

as:

```text
This project's import identity starts here.
```

---

## Beginner Module Names

For practice projects, these are fine:

```text
example.com/garage-app
github.com/yourname/garage-app
academy.local/garage-app
```

Use a real repository path when the module will live in a real repository.

For local-only exercises, `example.com/...` is okay.

---

## Add A Package Inside Your Module

Folder:

```text
garage-app/
  go.mod
  main.go
  garage/
    status.go
```

`garage/status.go`:

```go
package garage

func Status(speed int) string {
    if speed > 120 {
        return "warning"
    }

    return "ok"
}
```

`main.go`:

```go
package main

import (
    "fmt"

    "example.com/garage-app/garage"
)

func main() {
    fmt.Println(garage.Status(90))
}
```

Run:

```bash
go run .
```

---

## Imports

An import path tells Go which package you want.

Standard library:

```go
import "fmt"
```

Local package inside your module:

```go
import "example.com/garage-app/garage"
```

External package:

```go
import "github.com/google/uuid"
```

Notice:

```text
The package name used in code is usually the final package declaration, not
necessarily the final folder name.
```

Most of the time those match, and beginners should keep them matching.

---

## Add An External Dependency

Example:

```bash
go get github.com/google/uuid
```

Then use it:

```go
package main

import (
    "fmt"

    "github.com/google/uuid"
)

func main() {
    id := uuid.NewString()
    fmt.Println(id)
}
```

This updates `go.mod` and creates or updates `go.sum`.

---

## What `go.sum` Does

`go.sum` stores cryptographic checksums for downloaded module versions.

Plain language:

```text
go.sum helps Go verify that dependencies are the versions it expects.
```

Commit both:

- `go.mod`
- `go.sum`

Do not hand-edit `go.sum` while learning.

---

## `go mod tidy`

Run:

```bash
go mod tidy
```

It:

- adds missing dependencies needed by imports
- removes dependencies no longer imported
- updates `go.sum`

Run it after:

- adding imports
- removing imports
- deleting packages
- before committing dependency changes

---

## Required Versus Indirect Dependencies

You may see:

```go
require github.com/some/library v1.2.3

require github.com/some/helper v0.4.0 // indirect
```

Direct dependency:

```text
Your code imports it.
```

Indirect dependency:

```text
One of your dependencies needs it.
```

Do not panic when indirect dependencies appear. Let Go manage them.

---

## Beginner Dependency Rules

Use the standard library first when it is enough.

Add a dependency when:

- it solves a real problem
- it is maintained
- it has documentation
- it does not make your program harder to understand
- you can explain why you need it

Avoid:

- adding libraries before you understand the problem
- copying random `go get` commands without reading package docs
- committing mysterious dependency changes without `go mod tidy`

---

## Mini Project: Two-Package Module

Build:

```text
garage-app/
  go.mod
  main.go
  garage/
    status.go
    status_test.go
```

Commands:

```bash
go mod init example.com/garage-app
go test ./...
go run .
go mod tidy
```

`garage/status_test.go`:

```go
package garage

import "testing"

func TestStatus(t *testing.T) {
    got := Status(130)
    want := "warning"

    if got != want {
        t.Fatalf("Status(130) = %q, want %q", got, want)
    }
}
```

Goal:

```text
You can explain how main imports garage through the module path.
```

---

## Chapter Checkpoint

You should now be able to answer:

- What is a Go module?
- What does `go.mod` store?
- What does `go.sum` store?
- Why does the module path matter for imports?
- What does `go get` do?
- What does `go mod tidy` do?
- When should you avoid adding a dependency?

---

## Recap

- A module is the project boundary for Go dependencies.
- `go.mod` declares module identity and required modules.
- `go.sum` verifies dependency versions.
- Local package imports start with your module path.
- `go mod tidy` keeps dependency files accurate.
- Dependencies should solve real problems, not decorate the project.

## Try It Yourself

Create a module with:

- `main.go`
- one reusable package folder
- one test file
- a clean `go test ./...`
- a clean `go mod tidy`

Then explain every line of `go.mod`.

---

[**Next ->** Testing, Formatting, And Documentation Basics](./06-testing-formatting-and-documentation-basics.md)  
[**<- Previous** Go Command Workflow](./04-go-command-workflow.md)
