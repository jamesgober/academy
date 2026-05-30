<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 03](./README.md)

---

# Common Pointer Mistakes

> Most pointer pain comes from a few repeated mistakes. Learn the mistake
> patterns and pointers become much less spooky.

**You will learn:**
- The pointer mistakes beginners make most often
- How to recognize uninitialized, null, dangling, and wrong-type pointer bugs
- Why array bounds and pointer arithmetic are dangerous
- How to debug pointer code with a checklist instead of guessing

**Before this page, you should know:** [Function Arguments And Pointers](./04-function-arguments-and-pointers.md)

---

## Mistake 1: Uninitialized Pointer

Bad:

```c
int *ptr;
*ptr = 5;
```

`ptr` has an indeterminate value. It does not point to a valid `int`.

Safer:

```c
int value = 0;
int *ptr = &value;
*ptr = 5;
```

or:

```c
int *ptr = NULL;
```

Rule:

```text
Every pointer should start as either a valid address or NULL.
```

---

## Mistake 2: Dereferencing `NULL`

Bad:

```c
int *score = NULL;
printf("%d\n", *score);
```

`NULL` means "points to nothing."

Check first when null is possible:

```c
if (score != NULL) {
    printf("%d\n", *score);
}
```

Better function:

```c
void print_score(const int *score) {
    if (score == NULL) {
        printf("no score\n");
        return;
    }

    printf("%d\n", *score);
}
```

---

## Mistake 3: Confusing Pointer With Pointed-To Value

```c
int value = 10;
int *ptr = &value;
```

Different things:

```c
printf("%p\n", (void *)ptr); // address stored in ptr
printf("%d\n", *ptr);        // int value at that address
```

Question to ask:

```text
Do I need the address, or do I need the value at the address?
```

Address:

```c
ptr
```

Value:

```c
*ptr
```

---

## Mistake 4: Forgetting `&` When Calling A Pointer Function

Function:

```c
void reset(int *value) {
    if (value != NULL) {
        *value = 0;
    }
}
```

Wrong:

```c
int count = 5;
reset(count);
```

Correct:

```c
int count = 5;
reset(&count);
```

The function expects an address. `&count` gives the address.

---

## Mistake 5: Using A Pointer After Scope Ends

Bad:

```c
int *bad_pointer(void) {
    int local = 42;
    return &local;
}
```

`local` stops existing when the function returns.

The returned pointer dangles.

Better:

```c
int get_value(void) {
    return 42;
}
```

or allocate with clear ownership:

```c
int *make_value(void) {
    int *value = malloc(sizeof *value);
    if (value == NULL) {
        return NULL;
    }

    *value = 42;
    return value;
}
```

Caller must later call `free`.

---

## Mistake 6: Array Out Of Bounds

```c
int numbers[3] = {10, 20, 30};

printf("%d\n", numbers[3]); // bug
```

Valid indexes:

```text
0, 1, 2
```

Index `3` is outside the array.

C does not automatically stop you.

Loop safely:

```c
size_t count = 3;

for (size_t i = 0; i < count; i++) {
    printf("%d\n", numbers[i]);
}
```

---

## Mistake 7: Pointer Arithmetic Without A Plan

This is valid C:

```c
int numbers[3] = {10, 20, 30};
int *ptr = numbers;

printf("%d\n", *(ptr + 1));
```

It prints:

```text
20
```

But pointer arithmetic is easy to misuse. Prefer array indexing while learning:

```c
printf("%d\n", numbers[1]);
```

Beginner rule:

> Use indexes first. Learn pointer arithmetic only after arrays and bounds feel
> solid.

---

## Mistake 8: Wrong `sizeof`

Risky:

```c
int *numbers = malloc(count * sizeof(int *));
```

This allocates based on pointer size, not int size.

Better:

```c
int *numbers = malloc(count * sizeof *numbers);
```

Why this is good:

```text
sizeof *numbers means size of the thing numbers points to.
If the type changes later, allocation stays correct.
```

---

## Mistake 9: Ignoring Ownership

Whenever a pointer points to heap memory, ask:

```text
Who owns this memory?
Who frees it?
Can someone else still use it after free?
```

If the answer is unclear, the code is dangerous.

Good comment:

```c
/* Caller owns returned buffer and must free it. */
int *make_buffer(size_t count);
```

Bad comment:

```c
/* Returns some memory. */
```

Ownership is not optional in C. It is your job.

---

## Pointer Debugging Checklist

When pointer code misbehaves:

```text
1. Was the pointer initialized?
2. Can it be NULL?
3. Was it checked before dereference?
4. Does it point to an object that is still alive?
5. Was the memory already freed?
6. Is the type correct?
7. Is an array index out of bounds?
8. Is the allocation size correct?
9. Is ownership clear?
```

Use this checklist before randomly changing code.

---

## Mini Project: Fix The Pointer Bugs

Buggy code:

```c
#include <stdio.h>
#include <stdlib.h>

int *make_scores(size_t count) {
    int *scores = malloc(count * sizeof(int *));

    for (size_t i = 0; i <= count; i++) {
        scores[i] = (int)i * 10;
    }

    return scores;
}

int main(void) {
    int *scores;

    scores = make_scores(3);
    printf("%d\n", scores[3]);

    free(scores);
    printf("%d\n", scores[0]);

    return 0;
}
```

Find and fix:

- wrong `sizeof`
- missing allocation failure check
- loop goes out of bounds
- reads out of bounds
- use after `free`
- uninitialized pointer risk in `main`

---

## Chapter Checkpoint

You should now be able to answer:

- Why is `int *ptr; *ptr = 5;` unsafe?
- Why is dereferencing `NULL` unsafe?
- What is the difference between `ptr` and `*ptr`?
- Why is returning `&local` from a function wrong?
- Why does `numbers[3]` fail for a 3-element array?
- Why is `sizeof *ptr` often safer in allocation?
- What ownership question should every heap pointer answer?

---

## Recap

- Initialize pointers.
- Check nullable pointers.
- Know whether you need the pointer or the pointed-to value.
- Do not return addresses of local variables.
- Stay inside array bounds.
- Use `sizeof *ptr` for allocation.
- Write down ownership rules for heap memory.

## Try It Yourself

Take one pointer function you wrote earlier and add a comment block:

```c
/*
 * Reads:
 * Writes:
 * NULL allowed:
 * Ownership:
 * Caller responsibility:
 */
```

---

[**Next ->** Dynamic Memory: `malloc`, `calloc`, `free`](./06-dynamic-memory-malloc-calloc-free.md)  
[**<- Previous** Function Arguments And Pointers](./04-function-arguments-and-pointers.md)
