<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [C](../../README.md) · [Chapter 01](./README.md)

</div>

---

# Warnings, Errors, and Beginner Workflow

> In C, warnings matter. Ignoring them early teaches bad habits fast.

**You will learn:**
- The difference between warnings and errors
- Why warnings deserve attention
- A simple beginner-safe compile loop

**Before this page, you should know:** [What Source Files and Executables Are](./04-what-source-files-and-executables-are.md)

---

## Errors versus warnings

- **error**: the compiler refuses to build
- **warning**: the compiler builds, but it sees something suspicious

## Beginner workflow

1. compile with warnings enabled
2. read the output fully
3. fix errors first
4. fix warnings next
5. run the program only after the build is clean

## Why this matters in C

C gives you a lot of power and not much protection.
Warnings are one of the earliest guardrails you get.

> [!IMPORTANT]
> Treat warnings as important feedback, not background noise.

---

## Recap

- Errors stop the build.
- Warnings point at suspicious code.
- A clean build is part of writing trustworthy C.

## Try it yourself

Compile a tiny program with warnings enabled and write down what the compiler is trying to teach you.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← What Source Files and Executables Are](./04-what-source-files-and-executables-are.md) | [Chapter 01](./README.md) · [C](../../README.md) · [Home](../../../../README.md) | [Chapter 02 →](../02-syntax-and-control-flow/README.md) |

</div>
