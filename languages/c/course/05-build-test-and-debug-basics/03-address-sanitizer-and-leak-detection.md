<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 05](./README.md)

---

# Address Sanitizer And Leak Detection

> Sanitizers are runtime bug detectors. They make many invisible C memory errors
> loud, specific, and easier to fix.

**You will learn:**
- What AddressSanitizer does
- What UndefinedBehaviorSanitizer does
- How to compile with sanitizer flags
- How to read a report
- How leaks, use-after-free, and buffer overflow appear
- Why sanitizers do not replace ownership design

**Before this page, you should know:** [Runtime Debugging Basics](./02-runtime-debugging-basics.md)

---

## Sanitizer Build

GCC or Clang:

```bash
gcc -std=c17 -g -O1 \
    -fsanitize=address,undefined \
    -fno-omit-frame-pointer \
    main.c -o app_san
```

Run:

```bash
./app_san
```

What the flags mean:

| Flag | Meaning |
|---|---|
| `-fsanitize=address` | detect many memory access bugs |
| `-fsanitize=undefined` | detect many undefined behavior bugs |
| `-fno-omit-frame-pointer` | improve stack traces on many systems |
| `-g` | include debug information |
| `-O1` | light optimization often works well with sanitizers |

---

## Use-After-Free Example

Bad:

```c
#include <stdio.h>
#include <stdlib.h>

int main(void) {
    int *value = malloc(sizeof(*value));
    if (value == NULL) {
        return 1;
    }

    *value = 42;
    free(value);

    printf("%d\n", *value);
    return 0;
}
```

Sanitizer may report:

```text
ERROR: AddressSanitizer: heap-use-after-free
```

Meaning:

```text
The program used heap memory after it was freed.
```

Fix:

```c
printf("%d\n", *value);
free(value);
value = NULL;
```

Use the object before freeing it.

---

## Buffer Overflow Example

Bad:

```c
#include <stdio.h>

int main(void) {
    int values[3] = {1, 2, 3};

    printf("%d\n", values[3]);
    return 0;
}
```

Valid indexes:

```text
0, 1, 2
```

Index `3` is one past the end.

Sanitizer may report:

```text
ERROR: AddressSanitizer: stack-buffer-overflow
```

---

## Leak Example

Bad:

```c
#include <stdlib.h>

int main(void) {
    int *values = malloc(10 * sizeof(*values));
    if (values == NULL) {
        return 1;
    }

    values[0] = 10;
    return 0;
}
```

The allocation is never freed.

Fix:

```c
free(values);
values = NULL;
return 0;
```

Leak reporting depends on platform and sanitizer configuration, but the
ownership rule is universal:

```text
Every successful allocation needs a cleanup path.
```

---

## Reading A Report

Look for:

```text
1. bug type
2. invalid read/write line
3. allocation line
4. free line, if relevant
5. call stack
```

Example interpretation:

```text
allocated at line 6
freed at line 12
read at line 14
```

That tells the lifetime story:

```text
The pointer outlived the allocation.
```

---

## Sanitizers Are Not Proof

Sanitizers are excellent, but they do not prove a program is perfect.

They only check what runs.

If a branch is never executed during testing, sanitizer cannot report bugs in
that branch.

Use sanitizers with:

- focused tests
- edge-case inputs
- strict warnings
- ownership review

---

## Platform Notes

Sanitizer availability depends on:

- compiler
- operating system
- architecture
- runtime libraries

If sanitizer flags fail on your machine, do not skip memory discipline.

Still run:

```bash
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g main.c -o app
```

and review allocations/free paths manually.

---

## Chapter Checkpoint

You should now be able to answer:

- What does AddressSanitizer detect?
- What does UndefinedBehaviorSanitizer detect?
- What is use-after-free?
- What is buffer overflow?
- What is a leak?
- Why do stack traces matter?
- Why do sanitizers not prove the whole program is safe?

---

## Recap

- Sanitizers detect many runtime memory errors.
- Use sanitizer builds during development.
- Reports show bug type and stack traces.
- Use-after-free means memory was used after `free`.
- Buffer overflow means access went outside valid bounds.
- Leak means cleanup was missing.

## Try It Yourself

Create three tiny programs:

- one leak
- one out-of-bounds read
- one use-after-free

Run each with a sanitizer build and summarize the report in plain language.

---

[**Next ->** Memory-Issue Triage Workflow](./04-memory-issue-triage-workflow.md)  
[**<- Previous** Runtime Debugging Basics](./02-runtime-debugging-basics.md)
