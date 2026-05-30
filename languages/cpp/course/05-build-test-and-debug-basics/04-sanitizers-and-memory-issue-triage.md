<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

[Home](../../../../README.md) / [C++](../../README.md) / [Chapter 05](./README.md)

---

# Sanitizers And Memory-Issue Triage

> Sanitizers are runtime bug detectors. They do not replace good ownership
> design, but they are one of the best ways to catch memory mistakes while you
> are learning C++.

**You will learn:**
- What sanitizers do
- How to run AddressSanitizer and UndefinedBehaviorSanitizer
- How to read a sanitizer report
- How to triage use-after-free
- How to triage buffer overflow
- How to connect reports back to ownership

**Before this page, you should know:** [Avoiding Leaks And Lifetime Bugs](../04-memory-ownership-and-raii/04-avoiding-leaks-and-lifetime-bugs.md)

---

## What Sanitizers Are

A sanitizer adds runtime checks to your program.

Normal build:

```text
run program
maybe crash later
maybe corrupt memory silently
```

Sanitizer build:

```text
run program
detect certain bad behavior near where it happens
print a report
stop the program
```

---

## AddressSanitizer Command

GCC or Clang:

```bash
g++ -std=c++20 -g -O1 -fsanitize=address -fno-omit-frame-pointer main.cpp -o app_asan
./app_asan
```

Useful combined build:

```bash
g++ -std=c++20 -g -O1 -fsanitize=address,undefined -fno-omit-frame-pointer main.cpp -o app_san
./app_san
```

Meaning:

| Flag | Meaning |
|---|---|
| `-fsanitize=address` | detect many memory access errors |
| `-fsanitize=undefined` | detect many undefined behavior cases |
| `-fno-omit-frame-pointer` | often improves stack traces |
| `-g` | include debug information |

Windows note:

```text
AddressSanitizer support depends on compiler and toolchain version. Clang and
recent MSVC toolsets can support it, but exact commands differ.
```

---

## Use-After-Free Example

Bad program:

```cpp
#include <iostream>

int main() {
    int* value = new int(42);
    delete value;

    std::cout << *value << '\n';
}
```

Report fragment:

```text
ERROR: AddressSanitizer: heap-use-after-free
READ of size 4
```

Translation:

```text
The program read heap memory after that allocation was freed.
```

Root cause:

```text
value still stores an address,
but the object at that address is no longer alive.
```

Fix:

```cpp
#include <iostream>
#include <memory>

int main() {
    auto value = std::make_unique<int>(42);
    std::cout << *value << '\n';
}
```

---

## Buffer Overflow Example

Bad program:

```cpp
#include <vector>

int main() {
    std::vector<int> values{1, 2, 3};
    values[3] = 4;
}
```

Problem:

```text
Valid indexes are 0, 1, 2.
Index 3 is one past the end.
```

Sanitizer may report:

```text
ERROR: AddressSanitizer: heap-buffer-overflow
```

Safer during debugging:

```cpp
values.at(3) = 4;
```

`at()` checks bounds and throws an exception when the index is invalid.

Do not blindly replace all indexing with `at()` forever. Understand which
indexes are valid and why.

---

## Reading A Report

Look for:

```text
1. bug type
2. invalid read/write location
3. stack trace where bad access happened
4. stack trace where memory was allocated
5. stack trace where memory was freed, if relevant
```

Example:

```text
ERROR: AddressSanitizer: heap-use-after-free
    #0 main main.cpp:7
freed by thread T0 here:
    #0 operator delete
    #1 main main.cpp:5
previously allocated by thread T0 here:
    #0 operator new
    #1 main main.cpp:4
```

Read:

```text
allocated at line 4
freed at line 5
used after free at line 7
```

---

## Triage Flow

1. Identify the bug type.
2. Find the invalid access line.
3. Find where the object was allocated.
4. Find where ownership ended.
5. Ask who still has a stale pointer or reference.
6. Replace manual ownership with RAII when possible.
7. Rerun the sanitizer build.

---

## Sanitizers Are Not Magic

Sanitizers:

- catch many runtime bugs
- make invisible memory mistakes visible
- provide stack traces
- are excellent in development and CI

Sanitizers do not:

- prove the program has no bugs
- replace tests
- replace ownership design
- catch every possible issue

---

## Mini Project: Find And Fix Three Bugs

Start with:

```cpp
#include <iostream>
#include <vector>

int main() {
    int* value = new int(10);
    delete value;
    std::cout << *value << '\n';

    std::vector<int> scores{1, 2, 3};
    std::cout << scores[3] << '\n';

    int* leak = new int(99);
}
```

Build with sanitizer.

Fix it:

```cpp
#include <iostream>
#include <memory>
#include <vector>

int main() {
    auto value = std::make_unique<int>(10);
    std::cout << *value << '\n';

    std::vector<int> scores{1, 2, 3};

    if (scores.size() > 2) {
        std::cout << scores[2] << '\n';
    }

    auto no_leak = std::make_unique<int>(99);
    std::cout << *no_leak << '\n';
}
```

Then explain:

- which line was use-after-free
- which line was out-of-bounds
- which line leaked
- how RAII fixed the leak

---

## Chapter Checkpoint

You should now be able to answer:

- What does AddressSanitizer detect?
- What does UndefinedBehaviorSanitizer detect?
- What is use-after-free?
- What is heap-buffer-overflow?
- Why do stack traces matter?
- Why should you connect reports to ownership?
- Why do sanitizers not replace tests?

---

## Recap

- Sanitizers catch many runtime bugs.
- AddressSanitizer is especially useful for memory access errors.
- UndefinedBehaviorSanitizer catches many undefined behavior cases.
- Reports usually show where memory was allocated, freed, and misused.
- The real fix is usually an ownership or bounds fix.
- Rerun sanitizers after fixing the root cause.

## Try It Yourself

Take your Chapter 04 inventory project and run it with:

```bash
g++ -std=c++20 -g -O1 -fsanitize=address,undefined -fno-omit-frame-pointer main.cpp -o inventory_san
./inventory_san
```

If your toolchain does not support sanitizers, still compile with strict
warnings and review every ownership decision manually.

---

[**Next ->** Chapter 05 Capstone Project](./05-chapter-05-capstone-project.md)  
[**<- Previous** Debugging, Errors, And Warning Navigation](./03-debugging-errors-and-warning-navigation.md)
