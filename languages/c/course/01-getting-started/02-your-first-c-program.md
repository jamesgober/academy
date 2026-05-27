<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [C](../../README.md) · [Chapter 01](./README.md)

</div>

---

# Your First C Program

> Start with one tiny C file and understand every line.

**You will learn:**
- What `main` is
- Why `#include <stdio.h>` appears
- How `printf` produces output

**Before this page, you should know:** [Installing a C Compiler](./01-installing-a-c-compiler.md)

---

## First program

```c
#include <stdio.h>

int main(void) {
    printf("Hello, C\n");
    return 0;
}
```

## Read it slowly

- `#include <stdio.h>` makes input/output tools available
- `int main(void)` defines the program entry point
- `printf(...)` prints text
- `return 0;` says the program finished successfully

> [!NOTE]
> Beginners often copy this file before they understand it. Read it line by line first.

---

## Recap

- C programs start in `main`.
- `stdio.h` provides `printf`.
- `return 0` means success.

## Try it yourself

Change the text to print your name and favorite tool.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Installing a C Compiler](./01-installing-a-c-compiler.md) | [Chapter 01](./README.md) · [C](../../README.md) · [Home](../../../../README.md) | [Compiling and Running Step by Step →](./03-compiling-and-running-step-by-step.md) |

</div>
