<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [C](../../README.md) · [Chapter 04](./README.md)

</div>

---

# Strings in C: `char` Arrays and Null Terminators

> C strings are character arrays that end with a special `\0` terminator byte.

**You will learn:**
- How C represents strings in memory
- Why the null terminator is required
- How string size and capacity differ

**Before this page, you should know:** [Arrays and Indexed Access](./02-arrays-and-indexed-access.md)

---

## String example

```c
char name[] = "Ava";
```

Memory contains:
- `'A'`
- `'v'`
- `'a'`
- `'\0'`

That final `\0` marks end-of-string.

## Why this matters

String functions rely on the terminator.
Missing terminator can cause reads beyond intended memory.

## Capacity versus content length

- capacity: allocated array size
- content length: characters before `\0`

Keep both in mind when writing or copying strings.

---

## Recap

- C strings are `char` arrays plus `\0` terminator.
- Missing terminators create memory bugs.
- Size management is part of correct string handling.

## Try it yourself

Create a char array with space for 10 characters and store a 4-character word.
Explain remaining capacity.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Arrays and Indexed Access](./02-arrays-and-indexed-access.md) | [Chapter 04](./README.md) · [C](../../README.md) · [Home](../../../../README.md) | [Safe String Handling Patterns →](./04-safe-string-handling-patterns.md) |

</div>
