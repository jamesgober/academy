<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Go](../../README.md) · [Chapter 03](./README.md)

</div>

---

# Project Layout and Module Boundaries

> Clean project structure makes growth easier and debugging faster.

**You will learn:**
- How to keep a small Go project organized
- Where modules and packages fit in the structure
- What early architecture smells look like

**Before this page, you should know:** [Interfaces in Plain Language](./04-interfaces-in-plain-language.md)

---

## Small project example

```text
car-app/
├── go.mod
├── main.go
└── garage/
    └── status.go
```

## What this layout says

- `main.go` runs the application
- `garage` holds reusable domain logic
- `go.mod` defines the project boundary

## Early architecture smells

- one giant file doing everything
- too many exported names
- packages that only exist because the folder tree looks clever
- interfaces added before the real problem exists

> [!IMPORTANT]
> Prefer clear, boring structure over premature architecture.

---

## Recap

- Project layout should reflect responsibility.
- Modules define the project boundary.
- Packages should exist for clarity, not decoration.

## Try it yourself

Sketch a two-package Go project where `main` calls into one reusable package.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Interfaces in Plain Language](./04-interfaces-in-plain-language.md) | [Chapter 03](./README.md) · [Go](../../README.md) · [Home](../../../../README.md) | [Chapter 03 Checkpoint →](./06-chapter-03-checkpoint.md) |

</div>
