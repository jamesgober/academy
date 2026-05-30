<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 02](./README.md)

---

# Variables, Values, and Basic Types

> Variables let C programs keep values and use them again later.

**You will learn:**
- What a variable is
- How C basic types look
- Why type matters in low-level languages

**Before this page, you should know:** [Chapter 01](../01-getting-started/README.md)

---

## A variable in plain language

A variable is a named place in the program that stores a value.

```c
int age = 30;
char grade = 'A';
```

## Common beginner types

- `int` for whole numbers
- `char` for one character
- `float` for decimal values
- `double` for larger decimal precision

## Why type matters

C does not guess as much for you as higher-level languages do.
You need to be explicit.
That is part of why C teaches the machine model well.

> [!IMPORTANT]
> In C, type is not decoration. It affects memory layout and behavior.

---

## Recap

- Variables store values.
- C uses explicit types.
- Type affects how the program understands data.

## Try it yourself

Create one `int`, one `char`, and one `float`, then print them with `printf`.

---

[**Next ->** Functions, Parameters, and Return Values](./02-functions-parameters-and-return-values.md)
[**<- Previous** Chapter 01](../01-getting-started/README.md)


