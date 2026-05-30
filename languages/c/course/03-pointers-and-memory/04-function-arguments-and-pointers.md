<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 03](./README.md)

---

# Function Arguments And Pointers

> C passes function arguments by value. If a function needs to change the
> caller's variable, pass the variable's address.

**You will learn:**
- What pass-by-value means
- Why changing a parameter does not change the caller's variable
- How pointer parameters let functions modify caller data
- How to use pointers for output values
- How arrays relate to pointer parameters
- How to design pointer-taking functions safely

**Before this page, you should know:** [Dereferencing Without Hand-Waving](./03-dereferencing-without-hand-waving.md)

---

## C Arguments Are Copied

```c
#include <stdio.h>

void add_one(int value) {
    value = value + 1;
}

int main(void) {
    int score = 10;

    add_one(score);

    printf("%d\n", score);
    return 0;
}
```

Output:

```text
10
```

Why?

`add_one` receives a copy.

Visual model:

```text
main score:       10
add_one value:    10  <- copy

value changes to 11
score stays 10
```

---

## Pass The Address To Modify The Original

```c
void add_one(int *value_ptr) {
    if (value_ptr == NULL) {
        return;
    }

    *value_ptr = *value_ptr + 1;
}
```

Call:

```c
int score = 10;
add_one(&score);
```

Now:

```text
&score gives address of score.
value_ptr stores that address.
*value_ptr reaches score.
assignment changes score.
```

Full program:

```c
#include <stdio.h>

void add_one(int *value_ptr) {
    if (value_ptr == NULL) {
        return;
    }

    *value_ptr = *value_ptr + 1;
}

int main(void) {
    int score = 10;

    add_one(&score);

    printf("%d\n", score);
    return 0;
}
```

Output:

```text
11
```

---

## Read-Only Pointer Parameters

If a function only reads through a pointer, use `const`.

```c
void print_score(const int *score_ptr) {
    if (score_ptr == NULL) {
        printf("no score\n");
        return;
    }

    printf("score: %d\n", *score_ptr);
}
```

This communicates:

```text
I need an address, but I will not change the int through it.
```

Call:

```c
int score = 10;
print_score(&score);
```

---

## Output Parameters

C functions can return only one direct return value.

Pointer parameters let you fill extra outputs.

Example:

```c
#include <stdbool.h>

bool divide(int numerator, int denominator, int *result_out) {
    if (result_out == NULL) {
        return false;
    }

    if (denominator == 0) {
        return false;
    }

    *result_out = numerator / denominator;
    return true;
}
```

Call:

```c
int result = 0;

if (divide(10, 2, &result)) {
    printf("result: %d\n", result);
} else {
    printf("division failed\n");
}
```

Naming convention:

```text
result_out
```

The `_out` suffix tells readers:

```text
This pointer is used to write an output value.
```

---

## Swap Example

Classic pointer function:

```c
void swap_ints(int *left, int *right) {
    if (left == NULL || right == NULL) {
        return;
    }

    int temp = *left;
    *left = *right;
    *right = temp;
}
```

Call:

```c
int a = 5;
int b = 9;

swap_ints(&a, &b);

printf("a=%d b=%d\n", a, b);
```

Output:

```text
a=9 b=5
```

The function modifies both caller variables through their addresses.

---

## Arrays As Parameters

When you pass an array to a function, it usually becomes a pointer to its first
element.

```c
void print_scores(const int scores[], size_t count) {
    for (size_t i = 0; i < count; i++) {
        printf("%d\n", scores[i]);
    }
}
```

This is equivalent as a parameter:

```c
void print_scores(const int *scores, size_t count)
```

Always pass the length too.

Bad:

```c
void print_scores(const int *scores) {
    // function does not know how many elements exist
}
```

Good:

```c
void print_scores(const int *scores, size_t count)
```

C arrays do not carry their length into functions.

---

## Validate Pointer Parameters

Pointer-taking functions should decide:

```text
Is NULL allowed?
If NULL is allowed, what happens?
If NULL is not allowed, how do we report the error?
```

Example:

```c
bool total_scores(const int *scores, size_t count, int *total_out) {
    if (scores == NULL || total_out == NULL) {
        return false;
    }

    int total = 0;
    for (size_t i = 0; i < count; i++) {
        total += scores[i];
    }

    *total_out = total;
    return true;
}
```

This function has a clear contract:

- `scores` must point to valid array data when `count > 0`
- `total_out` must point to an `int`
- returns `true` when it writes output
- returns `false` when it cannot

---

## Pointer Function Design Checklist

Before writing a pointer parameter, answer:

```text
Will the function read through the pointer?
Will the function write through the pointer?
Can the pointer be NULL?
Who owns the pointed-to memory?
How long must the pointed-to memory stay alive?
For arrays, where is the length?
For output parameters, what happens on failure?
```

If you cannot answer these, the function is not ready.

---

## Mini Project: Score Helpers

Write three functions:

```c
bool add_bonus(int *score, int bonus);
bool average_scores(const int *scores, size_t count, double *average_out);
void print_score(const int *score);
```

Rules:

- `add_bonus` returns `false` for `NULL` or negative bonus
- `average_scores` returns `false` for `NULL`, zero count, or `NULL` output
- `print_score` prints `no score` for `NULL`
- use `const` when a function only reads

Example:

```c
int scores[] = {80, 90, 100};
double average = 0.0;

if (average_scores(scores, 3, &average)) {
    printf("average: %.2f\n", average);
}
```

---

## Chapter Checkpoint

You should now be able to answer:

- What does pass-by-value mean?
- Why does changing a normal parameter not change the caller's variable?
- Why does `add_one(&score)` work?
- What does an output parameter do?
- Why should read-only pointer parameters use `const`?
- Why do array parameters need a length?
- What should pointer-taking functions document or check?

---

## Recap

- C copies normal function arguments.
- Passing an address lets a function reach the caller's variable.
- `&x` passes the address.
- `*ptr` uses the pointed-to value.
- Output parameters are common in C.
- Arrays passed to functions need explicit lengths.
- Pointer APIs need clear null and ownership rules.

## Try It Yourself

Write `bool clamp_int(int *value, int min, int max)`.

Requirements:

- return `false` if `value == NULL`
- if `*value < min`, set it to `min`
- if `*value > max`, set it to `max`
- otherwise leave it unchanged
- return `true` when the pointer was valid

---

[**Next ->** Common Pointer Mistakes](./05-common-pointer-mistakes.md)  
[**<- Previous** Dereferencing Without Hand-Waving](./03-dereferencing-without-hand-waving.md)
