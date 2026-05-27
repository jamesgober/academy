<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [C](../../README.md) · [Chapter 04](./README.md)

</div>

---

# Safe String Handling Patterns

> String bugs are memory bugs in C, so handling must be deliberate.

**You will learn:**
- How to avoid common unsafe copy/concat patterns
- Why buffer size must travel with string data
- Practical habits that reduce overflow risk

**Before this page, you should know:** [Strings in C: `char` Arrays and Null Terminators](./03-strings-in-c-char-arrays-and-null-terminators.md)

---

## Core safety rules

- Always know destination buffer size.
- Always reserve room for `\0` terminator.
- Prefer bounded operations over unbounded ones.
- Validate input lengths before copy.

## Safer copy sketch

```c
char dest[16];
const char *src = "hello";

snprintf(dest, sizeof(dest), "%s", src);
```

`snprintf` helps bound writes to destination size.

## Risky pattern to avoid

```c
strcpy(dest, src);
```

If `src` is longer than destination capacity, overflow happens.

> [!WARNING]
> Buffer overflows can corrupt memory and create security vulnerabilities.

---

## Recap

- String handling in C requires explicit size discipline.
- Bounded writes are safer than unbounded writes.
- Destination capacity must be part of function design.

## Try it yourself

Write a function that copies a name into a fixed-size buffer and returns an error code when the name is too long.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Strings in C: `char` Arrays and Null Terminators](./03-strings-in-c-char-arrays-and-null-terminators.md) | [Chapter 04](./README.md) · [C](../../README.md) · [Home](../../../../README.md) | [Struct and Array Design Patterns →](./05-struct-and-array-design-patterns.md) |

</div>
