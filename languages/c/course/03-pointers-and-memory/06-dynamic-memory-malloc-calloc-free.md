<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 03](./README.md)

---

# Dynamic Memory: `malloc`, `calloc`, `free`

> Dynamic memory gives your program flexible storage at runtime. In C, that
> flexibility comes with a bill: you must release what you allocate.

**You will learn:**
- What stack and heap mean in beginner terms
- How `malloc` allocates memory
- How `calloc` allocates zero-initialized memory
- Why allocation can fail
- How to use `free`
- How to design allocation and cleanup together

**Before this page, you should know:** [Common Pointer Mistakes](./05-common-pointer-mistakes.md)

---

## Stack Versus Heap

Beginner model:

```text
Stack
  Automatic storage tied to scope.
  Local variables usually live here.
  Cleaned up automatically when the function returns.

Heap
  Dynamic storage requested at runtime.
  Stays allocated until you free it.
  You manage the lifetime manually.
```

Stack example:

```c
void example(void) {
    int local = 10;
}
```

`local` is cleaned up when `example` returns.

Heap example:

```c
int *numbers = malloc(5 * sizeof *numbers);
```

The allocated memory stays allocated until:

```c
free(numbers);
```

---

## Include The Right Header

Memory allocation functions live in:

```c
#include <stdlib.h>
```

You usually also need:

```c
#include <stdio.h>
```

for printing.

---

## `malloc`

`malloc` requests a number of bytes.

```c
int *numbers = malloc(5 * sizeof *numbers);
```

Read it:

```text
Allocate enough memory for 5 ints.
Store the starting address in numbers.
```

Always check:

```c
if (numbers == NULL) {
    fprintf(stderr, "allocation failed\n");
    return 1;
}
```

Why?

Allocation can fail when memory is unavailable or the requested size is too
large.

---

## Fill Allocated Memory

`malloc` does not initialize the memory for you.

```c
for (size_t i = 0; i < 5; i++) {
    numbers[i] = (int)(i + 1) * 10;
}
```

Then:

```c
for (size_t i = 0; i < 5; i++) {
    printf("%d\n", numbers[i]);
}
```

Do not read uninitialized allocated memory.

Bad:

```c
int *numbers = malloc(5 * sizeof *numbers);
printf("%d\n", numbers[0]); // bug if allocation succeeded but value not set
```

---

## `calloc`

`calloc` takes count and element size:

```c
int *numbers = calloc(5, sizeof *numbers);
```

Read it:

```text
Allocate 5 int-sized slots and set their bytes to zero.
```

Check:

```c
if (numbers == NULL) {
    fprintf(stderr, "allocation failed\n");
    return 1;
}
```

For integer arrays, zeroed bytes usually mean values start as `0`.

Use `calloc` when zero-initialized storage is useful.

Use `malloc` when you will immediately fill every element yourself.

---

## `free`

Release heap memory:

```c
free(numbers);
numbers = NULL;
```

`free` returns nothing.

After `free(numbers)`, the memory is no longer yours.

Setting the pointer to `NULL` prevents accidental reuse through that variable.

Important:

```c
free(NULL);
```

is safe. It does nothing.

---

## Full Example

```c
#include <stdio.h>
#include <stdlib.h>

int main(void) {
    size_t count = 5;
    int *numbers = malloc(count * sizeof *numbers);

    if (numbers == NULL) {
        fprintf(stderr, "allocation failed\n");
        return 1;
    }

    for (size_t i = 0; i < count; i++) {
        numbers[i] = (int)(i + 1) * 10;
    }

    for (size_t i = 0; i < count; i++) {
        printf("%d\n", numbers[i]);
    }

    free(numbers);
    numbers = NULL;

    return 0;
}
```

Compile with warnings:

```bash
cc -Wall -Wextra -Wpedantic -std=c17 main.c -o main
```

Run:

```bash
./main
```

On Windows with MSVC-style toolchain, your compile command may differ.

---

## Allocation Size Overflow

This can overflow:

```c
count * sizeof *numbers
```

For beginner exercises with small counts, it is okay.

For robust code, check before multiplying:

```c
if (count > SIZE_MAX / sizeof *numbers) {
    return 1;
}
```

You need:

```c
#include <stdint.h>
```

or:

```c
#include <stdint.h>
#include <stddef.h>
```

depending on the types used.

The main idea:

> If the size calculation wraps around, you may allocate too little memory and
> write past the allocation.

---

## Ownership Rule

Every allocation needs an ownership sentence.

Example:

```c
/* Caller owns returned buffer and must free it. */
int *make_numbers(size_t count);
```

Function:

```c
int *make_numbers(size_t count) {
    int *numbers = malloc(count * sizeof *numbers);
    if (numbers == NULL) {
        return NULL;
    }

    for (size_t i = 0; i < count; i++) {
        numbers[i] = (int)i;
    }

    return numbers;
}
```

Caller:

```c
int *numbers = make_numbers(5);
if (numbers == NULL) {
    return 1;
}

/* use numbers */

free(numbers);
numbers = NULL;
```

---

## Cleanup With One Exit Path

When a function has multiple resources, one cleanup path can reduce leaks.

```c
int run(void) {
    int status = 1;
    int *numbers = malloc(5 * sizeof *numbers);

    if (numbers == NULL) {
        goto cleanup;
    }

    /* use numbers */

    status = 0;

cleanup:
    free(numbers);
    return status;
}
```

Some teams avoid `goto`; many C codebases use it for cleanup. The key is not the
keyword. The key is that every path releases owned resources.

---

## Mini Project: Heap Score Buffer

Write a program that:

- asks for a count constant in code, such as `size_t count = 5`
- allocates an `int` array with `malloc`
- checks for allocation failure
- fills the array
- prints the array
- frees the array
- sets the pointer to `NULL`

Then rewrite it using `calloc`.

Compare:

```text
Which version needs an explicit fill before reading?
Which version starts as zero?
Where does free happen?
```

---

## Chapter Checkpoint

You should now be able to answer:

- What is the beginner difference between stack and heap?
- What does `malloc` return?
- Why must you check for `NULL`?
- What does `calloc` do differently from `malloc`?
- Why should you use `sizeof *ptr`?
- What does `free` do?
- Why should allocation and cleanup be designed together?

---

## Recap

- `malloc` allocates raw heap memory.
- `calloc` allocates and zero-initializes memory.
- Allocation can fail and return `NULL`.
- `free` releases heap memory.
- Do not use memory after freeing it.
- Every heap allocation needs a clear owner and cleanup path.

## Try It Yourself

Create a function:

```c
int *make_sequence(size_t count);
```

Rules:

- return `NULL` when allocation fails
- fill values from `1` through `count`
- document that the caller must `free`
- write a `main` that uses and frees the result

---

[**Next ->** Avoiding Leaks, Dangling Pointers, And Double Free](./07-avoiding-leaks-dangling-pointers-and-double-free.md)  
[**<- Previous** Common Pointer Mistakes](./05-common-pointer-mistakes.md)
