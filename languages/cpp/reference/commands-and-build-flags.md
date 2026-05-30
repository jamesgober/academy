# Commands And Build Flags

[Reference Index](./README.md) / [C++](../README.md)

---

## Related Lessons

- [Compiler Flags And Build Discipline](../course/05-build-test-and-debug-basics/01-compiler-flags-and-build-discipline.md)
- [Tests And Assertions In C++](../course/05-build-test-and-debug-basics/02-tests-and-assertions-in-cpp.md)
- [Debugging, Errors, And Warning Navigation](../course/05-build-test-and-debug-basics/03-debugging-errors-and-warning-navigation.md)
- [Sanitizers And Memory-Issue Triage](../course/05-build-test-and-debug-basics/04-sanitizers-and-memory-issue-triage.md)
- [Capstone Project](../course/05-build-test-and-debug-basics/05-chapter-05-capstone-project.md)

---

## GCC / Clang Basic Compile

```bash
g++ -std=c++20 -Wall -Wextra -Wpedantic -g main.cpp -o app
```

Run:

```bash
./app
```

On Windows PowerShell:

```powershell
.\app.exe
```

---

## MSVC Basic Compile

```powershell
cl /std:c++20 /W4 /EHsc /Zi main.cpp
```

Run:

```powershell
.\main.exe
```

---

## Flag Reference

| Flag | Toolchain | Meaning |
|---|---|---|
| `-std=c++20` | GCC/Clang | use C++20 |
| `/std:c++20` | MSVC | use C++20 |
| `-Wall -Wextra -Wpedantic` | GCC/Clang | strong warnings |
| `/W4` | MSVC | strong warnings |
| `-Werror` | GCC/Clang | warnings become errors |
| `/WX` | MSVC | warnings become errors |
| `-g` | GCC/Clang | debug information |
| `/Zi` | MSVC | debug information |
| `-O0` | GCC/Clang | no optimization, easier debugging |
| `/Od` | MSVC | no optimization |
| `-O2` | GCC/Clang | optimized release-style build |
| `/O2` | MSVC | optimized release-style build |
| `-DNDEBUG` | GCC/Clang | disable standard `assert` |
| `/DNDEBUG` | MSVC | disable standard `assert` |

---

## Multi-File Compile

```bash
g++ -std=c++20 -Wall -Wextra -Wpedantic -g main.cpp inventory.cpp -o inventory
```

Important:

```text
Compile .cpp files.
Headers are included by .cpp files.
Do not compile headers as separate programs.
```

---

## Tests

```bash
g++ -std=c++20 -Wall -Wextra -Wpedantic -g inventory_tests.cpp inventory.cpp -o inventory_tests
./inventory_tests
```

Use the same implementation files that the app uses.

---

## Warnings-As-Errors Gate

```bash
g++ -std=c++20 -Wall -Wextra -Wpedantic -Werror -g main.cpp inventory.cpp -o inventory
```

Use this when:

- preparing a final exercise
- checking release readiness
- running CI

Notice:

```text
Warnings can differ by compiler. Keep warnings clean, but expect toolchain
differences when switching compilers.
```

---

## Sanitizer Build

```bash
g++ -std=c++20 -g -O1 -fsanitize=address,undefined -fno-omit-frame-pointer main.cpp -o app_san
./app_san
```

Use this to catch many memory and undefined-behavior bugs.

Notice:

```text
Sanitizer support depends on compiler, platform, and version.
```

---

## Debug Build

```bash
g++ -std=c++20 -g -O0 -Wall -Wextra -Wpedantic main.cpp -o app_debug
```

Use while stepping through code or inspecting runtime behavior.

---

## Release-Style Build

```bash
g++ -std=c++20 -O2 -DNDEBUG -Wall -Wextra -Wpedantic main.cpp -o app
```

Use after tests pass.

Notice:

```text
Release builds may optimize code in ways that make debugging harder.
```

---

## Common Errors

| Message fragment | Likely cause |
|---|---|
| `undefined reference` | implementation missing from compile/link command |
| `not declared in this scope` | missing include, typo, namespace issue |
| `no matching function` | wrong argument types or const mismatch |
| `multiple definition` | non-inline implementation duplicated in headers |
| `cannot open include file` | include path or filename issue |

---

[Reference Index](./README.md) / [C++](../README.md)
