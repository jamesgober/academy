<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 04](./README.md)

---

# Strings In C: `char` Arrays And Null Terminators

> A C string is not a special string object. It is a sequence of characters that
> ends with `'\0'`.

**You will learn:**
- How C represents strings
- Why the null terminator matters
- How capacity differs from length
- Why string functions can read too far
- How string literals differ from writable arrays

**Before this page, you should know:** [Arrays And Indexed Access](./02-arrays-and-indexed-access.md)

---

## C String Mental Model

```c
char name[] = "Ava";
```

Memory:

```text
Index:   0     1     2     3
Value:  'A'   'v'   'a'   '\0'
```

The final `'\0'` is the null terminator.

It marks where the string ends.

Without it, C string functions do not know where to stop.

---

## Length Versus Capacity

```c
char name[10] = "Ava";
```

Capacity:

```text
10 char slots
```

String length:

```text
3 visible characters before '\0'
```

Memory:

```text
Index:   0     1     2     3      4 ... 9
Value:  'A'   'v'   'a'   '\0'   unused/zero-initialized here
```

Always reserve space for the terminator.

Maximum visible characters in `char name[10]`:

```text
9
```

because one slot is needed for `'\0'`.

---

## Print A String

```c
#include <stdio.h>

int main(void) {
    char name[] = "Ava";

    printf("%s\n", name);
    return 0;
}
```

`%s` prints characters until it finds `'\0'`.

That is why missing terminators are dangerous.

---

## Character Array Not Automatically A String

This is not a valid C string:

```c
char letters[3] = {'A', 'v', 'a'};
```

There is no terminator.

This is a valid C string:

```c
char letters[4] = {'A', 'v', 'a', '\0'};
```

Or:

```c
char letters[] = "Ava";
```

String literal initialization adds the terminator for you.

---

## `strlen`

Use:

```c
#include <string.h>
```

Example:

```c
char name[10] = "Ava";
size_t length = strlen(name);
```

`strlen` counts characters before `'\0'`.

It does not count capacity.

```text
strlen(name) -> 3
sizeof name  -> 10
```

`strlen` expects a valid null-terminated string. If the terminator is missing,
it can read past the array.

---

## String Literals

```c
const char *message = "hello";
```

Treat string literals as read-only.

Do not do this:

```c
char *message = "hello";
message[0] = 'H'; // bug
```

Writable copy:

```c
char message[] = "hello";
message[0] = 'H';
```

Now `message` is an array you own and can modify.

---

## Empty String

```c
char empty[] = "";
```

Memory:

```text
Index:  0
Value: '\0'
```

An empty C string still needs a terminator.

---

## Mini Project: Inspect A String

```c
#include <stdio.h>
#include <string.h>

int main(void) {
    char word[8] = "cat";

    printf("word: %s\n", word);
    printf("length: %zu\n", strlen(word));
    printf("capacity: %zu\n", sizeof word);

    for (size_t i = 0; i < sizeof word; i++) {
        printf("index %zu: %d\n", i, word[i]);
    }

    return 0;
}
```

Notice where `0` appears. That is the null terminator byte.

---

## Chapter Checkpoint

You should now be able to answer:

- What is a C string?
- What does `'\0'` do?
- Why does `char name[10]` hold at most 9 visible characters?
- What is the difference between `strlen(name)` and `sizeof name`?
- Why is `char letters[3] = {'A','v','a'};` not a string?
- Why should string literals be treated as read-only?

---

## Recap

- C strings are null-terminated character arrays.
- `'\0'` marks the end.
- Capacity and string length are different.
- `%s` and `strlen` rely on the terminator.
- String literals should not be modified.
- Writable strings need writable arrays.

## Try It Yourself

Create `char label[12] = "sensor";`. Print its length, capacity, and each raw
byte value. Identify the terminator.

---

[**Next ->** Safe String Handling Patterns](./04-safe-string-handling-patterns.md)  
[**<- Previous** Arrays And Indexed Access](./02-arrays-and-indexed-access.md)
