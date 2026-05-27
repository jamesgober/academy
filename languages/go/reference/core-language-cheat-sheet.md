<h1 align="center">
    <img width="99" alt="Go logo" src="../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

<div align="center">

[Home](../../../README.md) · [Track](../README.md) · [Reference Index](./README.md)

</div>

---

# Core Language Cheat Sheet

> Fast lookup for early Go syntax and mental models.

## Common types

- `string` text
- `int` whole numbers
- `bool` true/false
- `float64` decimal numbers

## Integer and float range notes

- `int` is platform-sized (commonly 64-bit on modern 64-bit systems)
- use explicit widths when range matters: `int8`, `int16`, `int32`, `int64`
- unsigned variants: `uint8`, `uint16`, `uint32`, `uint64`
- `float32` and `float64` are IEEE-754 floating-point types

If a data boundary matters, use explicit-width types instead of plain `int`.

## String and rune reminders

- `string` is immutable byte sequence
- `byte` is alias for `uint8`
- `rune` is alias for `int32` and usually represents a Unicode code point

## Common structures

- `if` for branching
- `for` for repetition
- `[]T` for slices
- `map[K]V` for key-value lookup

## Quick reminders

- `:=` creates a new variable when type can be inferred
- Go has one loop keyword: `for`
- slices are used more often than arrays in everyday code

---

<div align="center">

[← Reference Index](./README.md) · [Track](../README.md) · [Home](../../../README.md)

</div>
