<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [C](../../README.md) · [Chapter 04](./README.md)

</div>

---

# Arrays and Indexed Access

> Arrays store a fixed number of same-type values contiguously in memory.

**You will learn:**
- How arrays are declared
- How indexing works
- Why bounds awareness matters in C

**Before this page, you should know:** [Structs and Grouped Data](./01-structs-and-grouped-data.md)

---

## Basic array

```c
int speeds[3] = {30, 60, 90};
```

## Indexing

```c
printf("%d\n", speeds[0]);
printf("%d\n", speeds[2]);
```

Indexes start at `0`.

## Boundary rule

Valid indexes for a 3-element array are `0`, `1`, and `2`.
Access outside that range is undefined behavior.

> [!WARNING]
> C does not automatically protect array bounds at runtime.

---

## Recap

- Arrays hold fixed-size same-type sequences.
- Indexing starts at zero.
- Bounds mistakes are serious memory issues in C.

## Try it yourself

Create an array of five integers and print all values in a loop.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Structs and Grouped Data](./01-structs-and-grouped-data.md) | [Chapter 04](./README.md) · [C](../../README.md) · [Home](../../../../README.md) | [Strings in C: `char` Arrays and Null Terminators →](./03-strings-in-c-char-arrays-and-null-terminators.md) |

</div>
