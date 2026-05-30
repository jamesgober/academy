# C Commands And Build Flags

[Reference Index](./README.md) / [C](../README.md)

---

## Related Lessons

- [Installing A C Compiler](../course/01-getting-started/01-installing-a-c-compiler.md)
- [Compiling And Running Step By Step](../course/01-getting-started/03-compiling-and-running-step-by-step.md)
- [Compiler Warnings And Strict Build Flags](../course/05-build-test-and-debug-basics/01-compiler-warnings-and-strict-build-flags.md)
- [Address Sanitizer And Leak Detection](../course/05-build-test-and-debug-basics/03-address-sanitizer-and-leak-detection.md)
- [Chapter 05 Capstone](../course/05-build-test-and-debug-basics/06-chapter-05-capstone.md)

---

## Basic Compile

GCC:

```bash
gcc main.c -o app
./app
```

Clang:

```bash
clang main.c -o app
./app
```

MSVC:

```powershell
cl main.c
.\main.exe
```

---

## Strict Warning Build

```bash
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o app
```

Meaning:

| Flag | Meaning |
|---|---|
| `-std=c17` | use the C17 language standard |
| `-Wall` | enable many common warnings |
| `-Wextra` | enable additional warnings |
| `-Wpedantic` | warn about non-standard extensions |
| `-Werror` | treat warnings as errors |
| `-o app` | output executable name |

Use `-Werror` for final quality gates. While exploring, it is okay to compile
without `-Werror`, fix warnings, then turn it back on.

---

## Debug Build

```bash
gcc -std=c17 -g -O0 -Wall -Wextra -Wpedantic main.c -o app_debug
```

Use when debugging.

| Flag | Meaning |
|---|---|
| `-g` | include debug symbols |
| `-O0` | disable optimization for easier debugging |

---

## Sanitizer Build

```bash
gcc -std=c17 -g -O1 -fsanitize=address,undefined -fno-omit-frame-pointer main.c -o app_san
./app_san
```

Sanitizers can catch:

- use-after-free
- buffer overflow
- double free
- some undefined behavior
- some leaks, depending on platform/toolchain

Notice:

```text
Sanitizer support depends on compiler, operating system, and version.
```

---

## Multi-File Build

```bash
gcc -std=c17 -Wall -Wextra -Wpedantic -g main.c sensor_log.c -o sensor_log
```

Compile `.c` files.

Headers are included by `.c` files.

Do not compile headers as standalone programs.

---

## Common Diagnostics

| Message fragment | Common cause |
|---|---|
| `undefined reference` | function declared but implementation not linked |
| `implicit declaration of function` | missing prototype/header |
| `expected ';'` | syntax error before or at this line |
| `unused variable` | variable declared but not used |
| `comparison of integer expressions of different signedness` | signed/unsigned mismatch |
| `segmentation fault` | invalid memory access at runtime |

---

## Local Quality Loop

```bash
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g main.c sensor_log.c -o app
./app

gcc -std=c17 -g -O1 -fsanitize=address,undefined -fno-omit-frame-pointer main.c sensor_log.c -o app_san
./app_san
```

Add tests when possible:

```bash
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g tests.c sensor_log.c -o tests
./tests
```

---

## Risk Notices

- Warnings often predict real bugs. Do not ignore them.
- `undefined reference` is usually a link/build command issue, not a C syntax issue.
- Sanitizers do not prove the program is perfect.
- Every successful allocation needs a cleanup path.
- `realloc` should be assigned to a temporary pointer first.

---

[Reference Index](./README.md) / [C](../README.md)
