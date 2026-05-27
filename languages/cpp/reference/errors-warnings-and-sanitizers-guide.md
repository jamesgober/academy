# Errors, Warnings, and Sanitizers Guide

## Error reading pattern

`file:line: error: message`

Example:

```text
main.cpp:18:9: error: no matching function for call to 'parse'
```

Read in this order:
1. file + line
2. failing operation
3. expected versus provided signature/types

## Warning reading pattern

`file:line: warning: message`

Example:

```text
main.cpp:42: warning: comparison of integer expressions of different signedness
```

Warnings often indicate future bugs even when output is produced.

## Strict warning compile example

```bash
g++ -std=c++20 -Wall -Wextra -Wpedantic -Werror main.cpp -o app
```

## Sanitizer workflow

1. build with sanitizer flags
2. run failing scenario
3. inspect stack trace
4. patch ownership/lifetime bug
5. rerun until clean

## Sanitizer build example

```bash
g++ -std=c++20 -g -O1 -fsanitize=address -fno-omit-frame-pointer main.cpp -o app
```

## Memory bug classes to classify quickly

- use-after-free
- out-of-bounds
- double delete/free
- leak

---

[← Reference Index](./README.md) · [C++](../README.md)
