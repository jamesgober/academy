# C Commands and Build Flags

Quick lookup for common C build and run commands.

## Typical commands

```bash
gcc main.c -o app
./app
```

## Strict warning build

```bash
gcc -Wall -Wextra -Wpedantic -Werror main.c -o app
```

## Debug and sanitizer build

```bash
gcc -g -O1 -fsanitize=address -fno-omit-frame-pointer main.c -o app
```

## Quick reminders

- `-Wall -Wextra -Wpedantic` increases warning coverage.
- `-Werror` converts warnings to errors.
- `-g` adds debug symbols for investigation.
- sanitizer flags help catch memory misuse at runtime.

---

[← Reference Index](./README.md) · [C](../README.md)
