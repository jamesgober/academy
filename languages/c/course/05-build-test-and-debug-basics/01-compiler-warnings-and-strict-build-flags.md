<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 05](./README.md)

---

# Compiler Warnings And Strict Build Flags

> In C, warnings are not decoration. A warning can be the compiler quietly saying:
> "This might become a crash, corrupted data, or a memory bug later."

**You will learn:**
- Why warnings matter more in C
- What strict build flags do
- How to read common warning categories
- When to use `-Werror`
- How to build multi-file programs
- How strict builds fit into daily workflow

**Before this page, you should know:** [Chapter 04](../04-structs-arrays-and-strings/README.md)

---

## Beginner Strict Build

```bash
gcc -std=c17 -Wall -Wextra -Wpedantic -g main.c -o app
```

Meaning:

| Flag | Meaning |
|---|---|
| `-std=c17` | use the C17 standard |
| `-Wall` | enable many common warnings |
| `-Wextra` | enable additional warnings |
| `-Wpedantic` | warn about non-standard extensions |
| `-g` | include debug symbols |
| `-o app` | output executable name |

Use this while learning.

---

## Warnings-As-Errors Gate

```bash
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g main.c -o app
```

`-Werror` turns warnings into build failures.

Use it when:

- finishing an exercise
- preparing a capstone
- running a quality gate
- checking work before commit

While experimenting, it is okay to temporarily remove `-Werror`, understand the
warning, fix it, then turn `-Werror` back on.

---

## Common Warning: Unused Variable

```c
int total = 0;
```

If `total` is never used, the compiler may warn.

Why it matters:

```text
Unused variables often mean unfinished logic, wrong variable names, or dead code.
```

Fix:

- use the variable
- remove it
- finish the missing logic

---

## Common Warning: Signed/Unsigned Comparison

```c
#include <stddef.h>

void print_items(int count) {
    size_t max = 10;

    if (count < max) {
    }
}
```

`count` is signed.

`max` is unsigned.

The comparison may behave surprisingly for negative values.

Better:

```c
if (count < 0) {
    return;
}

if ((size_t)count < max) {
}
```

Convert only after validating that the signed value is non-negative.

---

## Common Warning: Missing Prototype

If you call a function before C has seen its declaration, older C habits can
create confusing diagnostics.

Good:

```c
int add(int left, int right);

int main(void) {
    return add(2, 3);
}

int add(int left, int right) {
    return left + right;
}
```

In multi-file projects, put declarations in a header.

---

## Multi-File Strict Build

```bash
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g \
    main.c sensor_log.c \
    -o sensor_log
```

Compile `.c` files.

Headers are included by `.c` files.

Do not compile `.h` files as standalone programs.

---

## MSVC Approximation

In Developer PowerShell:

```powershell
cl /std:c17 /W4 /WX /Zi main.c
```

Meaning:

| Flag | Meaning |
|---|---|
| `/std:c17` | use C17 mode where supported |
| `/W4` | strong warnings |
| `/WX` | warnings as errors |
| `/Zi` | debug information |

Toolchains differ, but the principle is the same:

```text
Warnings should be understood and fixed.
```

---

## Daily Workflow

```bash
gcc -std=c17 -Wall -Wextra -Wpedantic -g main.c -o app
./app
```

Then final gate:

```bash
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g main.c -o app
```

For multi-file projects:

```bash
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g main.c module.c -o app
```

---

## Chapter Checkpoint

You should now be able to answer:

- What does `-Wall` do?
- What does `-Wextra` do?
- What does `-Wpedantic` do?
- What does `-Werror` do?
- Why can signed/unsigned warnings matter?
- Why should headers declare shared functions?
- Why do you compile `.c` files instead of `.h` files?

---

## Recap

- Warnings are early bug signals.
- Strict flags make the compiler more helpful.
- `-Werror` is a quality gate.
- Signed/unsigned warnings can hide real logic bugs.
- Multi-file programs compile source files and include headers.

## Try It Yourself

Write a tiny two-file program with `math_helpers.h`, `math_helpers.c`, and
`main.c`. Build it with strict warnings and fix every warning.

---

[**Next ->** Runtime Debugging Basics](./02-runtime-debugging-basics.md)  
[**<- Previous** Chapter 04](../04-structs-arrays-and-strings/README.md)
