<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 04](./README.md)

---

# Arrays And Indexed Access

> Arrays store a fixed number of same-type values next to each other in memory.
> In C, the array does not protect you from bad indexes.

**You will learn:**
- How arrays are declared and initialized
- How indexes work
- Why array bounds matter
- Why arrays and functions need explicit lengths
- How arrays relate to pointers without pretending they are identical

**Before this page, you should know:** [Structs And Grouped Data](./01-structs-and-grouped-data.md)

---

## Basic Array

```c
int speeds[3] = {30, 60, 90};
```

This creates three `int` values in one fixed-size array.

Visual model:

```text
Index:   0    1    2
Value:  30   60   90
```

Valid indexes:

```text
0, 1, 2
```

Invalid index:

```text
3
```

An array with 3 elements does not have index 3.

---

## Read Values

```c
printf("%d\n", speeds[0]);
printf("%d\n", speeds[2]);
```

Output:

```text
30
90
```

Indexing starts at zero because `speeds[0]` means:

```text
start at the beginning of the array
move 0 elements forward
```

`speeds[2]` means:

```text
start at the beginning
move 2 elements forward
```

---

## Write Values

```c
speeds[1] = 65;
```

Now:

```text
Index:   0    1    2
Value:  30   65   90
```

Array elements are variables. You can read and write them.

---

## Loop Through An Array

```c
#include <stdio.h>

int main(void) {
    int speeds[3] = {30, 60, 90};
    size_t count = 3;

    for (size_t i = 0; i < count; i++) {
        printf("%d\n", speeds[i]);
    }

    return 0;
}
```

Use:

```c
i < count
```

not:

```c
i <= count
```

For `count = 3`, valid indexes stop at `2`.

---

## Get Element Count With `sizeof`

Inside the same scope where the array is declared:

```c
int speeds[] = {30, 60, 90};
size_t count = sizeof speeds / sizeof speeds[0];
```

Meaning:

```text
total array bytes / one element bytes = element count
```

This works for real arrays in the same scope.

It does not work after the array is passed to a function as a pointer.

---

## Arrays In Functions Need Lengths

Bad:

```c
void print_speeds(const int speeds[]) {
    /* no reliable count here */
}
```

Good:

```c
void print_speeds(const int speeds[], size_t count) {
    if (speeds == NULL) {
        return;
    }

    for (size_t i = 0; i < count; i++) {
        printf("%d\n", speeds[i]);
    }
}
```

Call:

```c
int speeds[] = {30, 60, 90};
size_t count = sizeof speeds / sizeof speeds[0];

print_speeds(speeds, count);
```

Rule:

> When an array crosses a function boundary, send the length too.

---

## Array Parameter Is Pointer-Like

These parameter forms are equivalent:

```c
void print_speeds(const int speeds[], size_t count)
```

```c
void print_speeds(const int *speeds, size_t count)
```

Both receive a pointer to the first element.

That is why the function cannot use `sizeof speeds` to recover the original
array length.

Beginner wording:

```text
Arrays often decay to pointers when passed to functions.
```

Do not turn that into "arrays and pointers are the same thing." They are related,
not identical.

---

## Bounds Mistakes Are Memory Mistakes

Bad:

```c
int speeds[3] = {30, 60, 90};
printf("%d\n", speeds[3]);
```

C does not automatically throw a friendly exception.

This is undefined behavior.

It might:

- print random data
- seem to work
- crash
- corrupt something nearby

Use compiler warnings and sanitizers:

```bash
cc -Wall -Wextra -Wpedantic -std=c17 -fsanitize=address -g main.c -o main
```

---

## Mini Project: Score Array Helpers

Write:

```c
void print_scores(const int scores[], size_t count);
bool average_scores(const int scores[], size_t count, double *average_out);
```

Rules:

- `print_scores` does nothing if `scores == NULL`
- `average_scores` returns `false` for `NULL`, zero count, or `NULL` output
- both functions use `i < count`
- both functions use `const int scores[]` because they do not modify scores

---

## Chapter Checkpoint

You should now be able to answer:

- What indexes are valid for `int values[4]`?
- Why is `values[4]` invalid?
- How can you compute count with `sizeof` in the declaration scope?
- Why do array functions need a `count` parameter?
- How are array parameters pointer-like?
- Why are bounds errors memory bugs in C?

---

## Recap

- Arrays hold fixed-size same-type sequences.
- Indexing starts at zero.
- The last valid index is `count - 1`.
- C does not protect array bounds automatically.
- Pass array lengths to functions.
- Use `const` for array parameters you only read.

## Try It Yourself

Create an array of five integers. Write helper functions to print, total, and
average the values. Every helper should receive both the array and its count.

---

[**Next ->** Strings In C: `char` Arrays And Null Terminators](./03-strings-in-c-char-arrays-and-null-terminators.md)  
[**<- Previous** Structs And Grouped Data](./01-structs-and-grouped-data.md)
