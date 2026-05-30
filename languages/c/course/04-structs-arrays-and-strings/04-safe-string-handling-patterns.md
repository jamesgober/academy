<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 04](./README.md)

---

# Safe String Handling Patterns

> String bugs are memory bugs in C. Safe string code is mostly size discipline:
> know the destination capacity, leave room for `'\0'`, and check results.

**You will learn:**
- Why unbounded string functions are risky
- How to copy with `snprintf`
- How to read input with `fgets`
- How to remove a trailing newline
- How to design functions that receive destination buffers safely
- Why truncation must be detected

**Before this page, you should know:** [Strings In C: `char` Arrays And Null Terminators](./03-strings-in-c-char-arrays-and-null-terminators.md)

---

## The Golden Rule

Every string write needs:

```text
destination buffer
destination capacity
room for '\0'
result check
```

If you do not know the destination capacity, you cannot safely write a C string.

---

## Avoid Unbounded Copy

Risky:

```c
char dest[8];
const char *src = "this is too long";

strcpy(dest, src); // overflow
```

`strcpy` does not know `dest` capacity.

It keeps copying until it sees `'\0'` in `src`, even if `dest` is too small.

Overflow can corrupt memory.

---

## Copy With `snprintf`

```c
#include <stdio.h>

char dest[8];
const char *src = "hello";

int written = snprintf(dest, sizeof dest, "%s", src);
```

Check result:

```c
if (written < 0) {
    /* encoding or formatting error */
}

if ((size_t)written >= sizeof dest) {
    /* output was truncated */
}
```

`snprintf` writes at most the destination size and includes a terminator when
size is greater than zero.

Important:

```text
Bounded does not mean silently okay.
You still need to detect truncation.
```

---

## Build A Safe Copy Helper

```c
#include <stdbool.h>
#include <stdio.h>

bool copy_string(char *dest, size_t dest_size, const char *src) {
    if (dest == NULL || src == NULL || dest_size == 0) {
        return false;
    }

    int written = snprintf(dest, dest_size, "%s", src);
    if (written < 0) {
        return false;
    }

    if ((size_t)written >= dest_size) {
        return false;
    }

    return true;
}
```

This function has a clear contract:

- destination pointer must be valid
- destination size must be nonzero
- source pointer must be valid
- returns `false` if copy fails or truncates

---

## Concatenate Safely

Risky:

```c
strcat(dest, suffix);
```

Safer pattern:

```c
bool append_string(char *dest, size_t dest_size, const char *suffix) {
    if (dest == NULL || suffix == NULL || dest_size == 0) {
        return false;
    }

    size_t used = strlen(dest);
    if (used >= dest_size) {
        return false;
    }

    int written = snprintf(dest + used, dest_size - used, "%s", suffix);
    if (written < 0) {
        return false;
    }

    return (size_t)written < dest_size - used;
}
```

This checks how much space is already used before appending.

---

## Read Input With `fgets`

Risky:

```c
scanf("%s", name);
```

Better beginner pattern:

```c
char name[32];

if (fgets(name, sizeof name, stdin) == NULL) {
    printf("no input\n");
    return 1;
}
```

`fgets` receives the buffer and its capacity.

It may keep the newline if there is room.

Remove trailing newline:

```c
name[strcspn(name, "\n")] = '\0';
```

Requires:

```c
#include <string.h>
```

---

## Detect Input Truncation

If the input line is longer than the buffer, `fgets` reads only part of it.

Simple check:

```c
if (strchr(name, '\n') == NULL) {
    /* line may have been too long */
}
```

Then you may need to discard the rest of the line:

```c
int ch;
while ((ch = getchar()) != '\n' && ch != EOF) {
    /* discard */
}
```

Beginner note:

> Input handling is a real topic. Do not treat user input as automatically safe.

---

## Destination Buffer Function Pattern

Common C API shape:

```c
bool format_entry(
    char *dest,
    size_t dest_size,
    const char *name,
    int score
);
```

Implementation:

```c
bool format_entry(char *dest, size_t dest_size, const char *name, int score) {
    if (dest == NULL || name == NULL || dest_size == 0) {
        return false;
    }

    int written = snprintf(dest, dest_size, "%s: %d", name, score);
    if (written < 0) {
        return false;
    }

    return (size_t)written < dest_size;
}
```

Caller:

```c
char line[64];

if (format_entry(line, sizeof line, "sensor", 42)) {
    printf("%s\n", line);
} else {
    printf("could not format entry\n");
}
```

---

## Risk Notices

| Pattern | Risk |
|---|---|
| `strcpy(dest, src)` | Destination overflow if `src` is too long |
| `strcat(dest, src)` | Destination overflow if remaining space is too small |
| `scanf("%s", buf)` | Can overflow without width |
| Ignoring `snprintf` result | Truncation may go unnoticed |
| Forgetting room for `'\0'` | Not a valid C string |
| Passing pointer without capacity | Function cannot know safe write limit |

---

## Mini Project: Safe Name Field

Write:

```c
bool set_name(char *dest, size_t dest_size, const char *name);
```

Rules:

- return `false` for `NULL`
- return `false` when `dest_size == 0`
- return `false` if name does not fit
- leave destination as a valid string when possible
- use `snprintf`

Test manually:

```c
char name[8];

printf("%d\n", set_name(name, sizeof name, "Ava"));
printf("%s\n", name);

printf("%d\n", set_name(name, sizeof name, "this name is too long"));
```

---

## Chapter Checkpoint

You should now be able to answer:

- Why is `strcpy` risky?
- What does `snprintf` need besides a destination pointer?
- How do you detect `snprintf` truncation?
- Why is `fgets` safer than unbounded input reads?
- Why might `fgets` leave a newline?
- Why should destination size travel with destination pointer?

---

## Recap

- C string writes require explicit capacity.
- Reserve room for `'\0'`.
- `snprintf` helps bound writes but must be checked.
- `fgets` is a safer beginner input pattern.
- Remove trailing newline deliberately.
- Design string functions with `dest` and `dest_size`.

## Try It Yourself

Write a program that asks for a username with `fgets`, removes the newline,
copies it into a smaller display buffer with `set_name`, and reports when the
display name is too long.

---

[**Next ->** Struct And Array Design Patterns](./05-struct-and-array-design-patterns.md)  
[**<- Previous** Strings In C: `char` Arrays And Null Terminators](./03-strings-in-c-char-arrays-and-null-terminators.md)
