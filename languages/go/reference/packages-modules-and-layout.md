<h1 align="center">
    <img width="99" alt="Go logo" src="../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

<div align="center">

[Home](../../../README.md) · [Track](../README.md) · [Reference Index](./README.md)

</div>

---

# Packages, Modules, and Layout Cheat Sheet

> Fast lookup for Go project structure terms.

## Core terms

- **file**: one `.go` source file
- **package**: a group of Go files that belong together
- **module**: the dependency and project boundary defined by `go.mod`
- **`package main`**: runnable application package
- **non-`main` package**: reusable code package

## Quick structure example

```text
hello-go/
├── go.mod
├── main.go
└── greeting/
    └── message.go
```

## Beginner reminders

- files in one folder usually share one package name
- package names should be short and clear
- `main` is for runnable apps, not reusable libraries
- `go.mod` belongs near the project root

## Quick diagnostic prompts

- is this folder supposed to be runnable or reusable?
- does every file in this folder use the correct package name?
- did I initialize a module yet?

---

<div align="center">

[← Reference Index](./README.md) · [Track](../README.md) · [Home](../../../README.md)

</div>
