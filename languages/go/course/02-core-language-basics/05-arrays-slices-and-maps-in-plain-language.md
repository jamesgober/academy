<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Go](../../README.md) · [Chapter 02](./README.md)

</div>

---

# Arrays, Slices, and Maps in Plain Language

> Go gives you different containers for different jobs.

**You will learn:**
- What arrays are
- Why slices are used more often in normal Go code
- What maps do well

**Before this page, you should know:** [Loops and Repetition in Go](./04-loops-and-repetition-in-go.md)

---

## Arrays

An array has a fixed size.

```go
var scores [3]int
```

That means exactly three integers.

## Slices

A slice is a flexible view over a sequence of values.

```go
names := []string{"Ana", "Bo", "Cam"}
```

Beginners will use slices much more often than arrays.

## Maps

A map stores key-value pairs.

```go
ages := map[string]int{
    "Ana": 21,
    "Bo": 19,
}
```

You use the key to get the value.

## Mental model

- array: fixed length list
- slice: flexible list
- map: lookup table

> [!TIP]
> If you are not sure whether to use an array or slice, the answer is usually slice.

---

## Recap

- Arrays are fixed length.
- Slices are the everyday list structure in Go.
- Maps pair keys with values.

## Try it yourself

Create one slice of three colors and one map from color name to ranking number.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Loops and Repetition in Go](./04-loops-and-repetition-in-go.md) | [Chapter 02](./README.md) · [Go](../../README.md) · [Home](../../../../README.md) | [Chapter 02 Checkpoint →](./06-chapter-02-checkpoint.md) |

</div>
