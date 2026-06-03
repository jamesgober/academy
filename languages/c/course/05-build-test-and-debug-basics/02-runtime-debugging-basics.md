<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 05](./README.md)

---

# Runtime Debugging Basics

> Build output tells you whether the compiler accepted your code. Runtime
> debugging tells you what your program actually did after it started running.

**You will learn:**
- How to reproduce a bug
- How to shrink a failing case
- What values to inspect
- How print debugging helps
- How a debugger helps
- How to reason about crashes in C

**Before this page, you should know:** [Compiler Warnings And Strict Build Flags](./01-compiler-warnings-and-strict-build-flags.md)

---

## Debugging Loop

Use this:

1. Reproduce the problem.
2. Write down expected behavior.
3. Write down actual behavior.
4. Shrink the input.
5. Inspect values near the failure.
6. Fix the smallest real cause.
7. Rebuild with strict warnings.
8. Rerun the scenario.

Random edits are how bugs hide.

---

## Print Debugging

Print important values:

```c
printf("count=%zu capacity=%zu\n", log->count, log->capacity);
```

Useful values to print:

- pointer addresses with `%p`
- counts and capacities
- indexes before array access
- string lengths
- allocation success/failure
- branch decisions

Pointer example:

```c
printf("items pointer=%p\n", (void *)log->items);
```

Cast to `void *` for `%p`.

---

## Crash Mental Model

A crash often means:

```text
The program touched memory it was not allowed to touch.
```

Common causes:

- null pointer dereference
- out-of-bounds array access
- use-after-free
- stack buffer overflow
- wrong string terminator assumptions

Start by asking:

```text
Which pointer or index was used at the crash line?
What made the program believe it was valid?
Was that belief checked?
```

---

## Example Bug: Out-Of-Bounds

Bad:

```c
#include <stdio.h>

int main(void) {
    int values[3] = {10, 20, 30};

    for (int i = 0; i <= 3; i++) {
        printf("%d\n", values[i]);
    }

    return 0;
}
```

Bug:

```text
Valid indexes are 0, 1, 2.
The loop also reads index 3.
```

Fix:

```c
for (int i = 0; i < 3; i++) {
    printf("%d\n", values[i]);
}
```

---

## Example Bug: Null Pointer

Bad:

```c
void print_first(int *values) {
    printf("%d\n", values[0]);
}
```

If caller passes `NULL`, this crashes.

Safer:

```c
void print_first(int *values, size_t count) {
    if (values == NULL || count == 0) {
        return;
    }

    printf("%d\n", values[0]);
}
```

In C, pass pointer plus length when working with arrays.

---

## Debugger Basics

With debug symbols:

```bash
gcc -std=c17 -g -O0 -Wall -Wextra -Wpedantic main.c -o app_debug
```

Common debugger actions:

| Action | Meaning |
|---|---|
| breakpoint | pause at a line |
| step over | run current line |
| step into | enter function call |
| continue | run until next breakpoint/crash |
| inspect variable | see current value |
| call stack | see how execution got here |

Tool names differ by platform: `gdb`, `lldb`, Visual Studio debugger, or IDE
debuggers. The concepts are the same.

---

## Shrinking A Bug

Instead of debugging the whole program:

```text
load file -> parse -> allocate -> transform -> print -> crash
```

shrink:

```text
Can parse one line?
Can allocate one item?
Can print one item?
Can print zero items?
```

Small reproductions make C bugs far easier to see.

---

## Chapter Checkpoint

You should now be able to answer:

- Why is reproduction step one?
- What values are useful to print?
- What does `%p` print?
- Why should arrays travel with lengths?
- What is an off-by-one array bug?
- What does a call stack show?
- Why should you shrink a failing case?

---

## Recap

- Runtime debugging checks real program behavior.
- Print debugging is useful when done intentionally.
- Crashes often involve invalid memory access.
- Arrays need lengths.
- Debuggers let you pause and inspect state.
- Shrinking a bug beats guessing.

## Try It Yourself

Introduce an off-by-one bug into a small array program. Then write:

- expected behavior
- actual behavior
- bad index
- fixed loop condition

---

[**Next ->** Address Sanitizer And Leak Detection](./03-address-sanitizer-and-leak-detection.md)  
[**<- Previous** Compiler Warnings And Strict Build Flags](./01-compiler-warnings-and-strict-build-flags.md)
