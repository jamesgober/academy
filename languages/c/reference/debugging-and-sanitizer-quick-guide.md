# C Debugging And Sanitizer Quick Guide

[Reference Index](./README.md) / [C](../README.md)

Use this page when a C program crashes, leaks memory, prints strange output, or
triggers sanitizer reports. For the lesson path, see [Runtime Debugging Basics](../course/05-build-test-and-debug-basics/02-runtime-debugging-basics.md) and [AddressSanitizer and Leak Detection](../course/05-build-test-and-debug-basics/03-address-sanitizer-and-leak-detection.md).

## First Triage Flow

```text
1. Reproduce the issue.
2. Rebuild with strict warnings.
3. Fix warnings first.
4. Run with sanitizer if memory is involved.
5. Read the first relevant report.
6. Fix the root cause.
7. Rebuild and rerun.
```

Do not patch random lines. Make the bug observable, then make the fix
observable.

## Strict Build

```bash
clang -std=c17 -Wall -Wextra -Wpedantic -g main.c -o app
```

or:

```bash
gcc -std=c17 -Wall -Wextra -Wpedantic -g main.c -o app
```

`-g` includes debug information. Warnings help catch bugs before runtime.

## AddressSanitizer Build

```bash
clang -std=c17 -Wall -Wextra -Wpedantic -g -O1 \
  -fsanitize=address -fno-omit-frame-pointer \
  main.c -o app
```

Run:

```bash
./app
```

AddressSanitizer can catch many memory errors:

- use after free
- heap buffer overflow
- stack buffer overflow
- double free
- leaks on many platforms

## Common Report Meanings

| Report Phrase | Meaning | Usual Fix Direction |
|---|---|---|
| `heap-buffer-overflow` | read/write beyond allocated heap memory | check allocation size and indexes |
| `stack-buffer-overflow` | read/write beyond local array | check local buffer capacity |
| `heap-use-after-free` | used memory after `free` | stop using pointer after ownership ends |
| `double-free` | freed same allocation twice | make ownership single and clear |
| `memory leaks` | allocation was not freed | add cleanup on every exit path |

## Print Debugging

For small programs, temporary prints can help:

```c
printf("index=%zu count=%zu\n", index, count);
```

Good debug prints include variable names and values.

Remove temporary debug prints before finalizing the lesson or program.

## Debugger Reminder

With debug info enabled, a debugger can:

- set breakpoints
- step line by line
- inspect variables
- inspect call stack

Useful when prints are too noisy or the bug depends on control flow.

## Ownership Trace

When debugging memory, write this down:

```text
allocation:
owner:
passed to:
freed at:
used after free?:
all failure paths free it?:
```

Most C memory bugs become clearer when ownership is written explicitly.

## Risk Notices

- Sanitizers are not a replacement for understanding ownership.
- A sanitizer report may point to where the bug exploded, not where it began.
- Do not ignore warnings just because the program runs.
- Do not use a pointer after `free`.
- Do not free memory you did not allocate or do not own.
- Do not return pointers to local stack arrays.

## Related References

- [C Commands and Build Flags](./c-commands-and-build-flags.md)
- [Memory Safety and Leak Prevention Checklist](./memory-safety-and-leak-prevention-checklist.md)
- [Pointers and Dynamic Memory Cheat Sheet](./pointers-and-dynamic-memory-cheat-sheet.md)

---

[Reference Index](./README.md) / [C](../README.md)
