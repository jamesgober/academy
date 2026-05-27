<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [C](../../README.md) · [Chapter 05](./README.md)

</div>

---

# Compiler Warnings and Strict Build Flags

> In C, warnings are often the first signal of real memory and correctness risks.

**You will learn:**
- Why warning levels matter
- Common strict flags
- How to treat warnings as part of quality, not optional noise

**Before this page, you should know:** [Chapter 04](../04-structs-arrays-and-strings/README.md)

---

## Typical strict compile command

```bash
gcc -Wall -Wextra -Wpedantic -Werror main.c -o app
```

Meaning:
- `-Wall -Wextra -Wpedantic` enables broad warnings
- `-Werror` treats warnings as errors

## Why this helps memory safety

Many pointer and buffer mistakes show up first as compiler warnings.
If warnings are ignored, defects survive longer and become harder to debug.

---

## Recap

- Strict warning flags catch issues early.
- Treating warnings as errors raises quality discipline.
- Cleaner builds reduce later debugging pain.

## Try it yourself

Compile one small program with and without strict flags and compare output.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Chapter 04](../04-structs-arrays-and-strings/README.md) | [Chapter 05](./README.md) · [C](../../README.md) · [Home](../../../../README.md) | [Runtime Debugging Basics →](./02-runtime-debugging-basics.md) |

</div>
