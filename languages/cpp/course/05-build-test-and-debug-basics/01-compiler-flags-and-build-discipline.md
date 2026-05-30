<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

[Home](../../../../README.md) / [C++](../../README.md) / [Chapter 05](./README.md)

---

# Compiler Flags And Build Discipline

> C++ gives you enormous control. Build discipline is how you keep that control
> from turning into mystery bugs. A clean build is not paperwork. It is the first
> test your program passes.

**You will learn:**
- What a compiler command really does
- Why language standards matter
- Which warnings to enable
- When to use `-Werror`
- How debug and release builds differ
- How to organize a tiny multi-file project
- How to read build commands without panic

**Before this page, you should know:** [Chapter 04: Memory, Ownership, And RAII](../04-memory-ownership-and-raii/README.md)

---

## The Build Pipeline

When you compile C++, several things happen:

```text
source code
  |
  | preprocess includes and macros
  v
translation units
  |
  | compile each .cpp file
  v
object files
  |
  | link object files and libraries
  v
executable
```

For one file, this can feel invisible:

```bash
g++ main.cpp -o app
```

For real projects, understanding the steps helps you debug errors faster.

---

## Start With A Strict Beginner Command

GCC or Clang:

```bash
g++ -std=c++20 -Wall -Wextra -Wpedantic -g main.cpp -o app
```

Meaning:

| Flag | Meaning |
|---|---|
| `-std=c++20` | use the C++20 language standard |
| `-Wall` | enable many common warnings |
| `-Wextra` | enable more warnings |
| `-Wpedantic` | warn about non-standard extensions |
| `-g` | include debug information |
| `main.cpp` | input source file |
| `-o app` | output executable name |

Run:

```bash
./app
```

On Windows PowerShell with MinGW or similar:

```powershell
.\app.exe
```

---

## MSVC Command

In a Developer PowerShell or Developer Command Prompt:

```powershell
cl /std:c++20 /W4 /EHsc /Zi main.cpp
```

Meaning:

| Flag | Meaning |
|---|---|
| `/std:c++20` | use C++20 |
| `/W4` | strong warning level |
| `/EHsc` | standard C++ exception handling behavior |
| `/Zi` | debug information |

MSVC produces `main.exe` by default:

```powershell
.\main.exe
```

---

## Warning Levels Are Teaching Tools

Warnings tell you:

```text
This code compiles, but something looks suspicious.
```

Example:

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> values{1, 2, 3};

    for (int i = 0; i < values.size(); ++i) {
        std::cout << values[i] << '\n';
    }
}
```

Some compilers warn because `i` is signed and `values.size()` is unsigned.

One fix:

```cpp
for (std::size_t i = 0; i < values.size(); ++i) {
    std::cout << values[i] << '\n';
}
```

Even better when you do not need the index:

```cpp
for (int value : values) {
    std::cout << value << '\n';
}
```

---

## Should You Use `-Werror`?

`-Werror` turns warnings into errors.

```bash
g++ -std=c++20 -Wall -Wextra -Wpedantic -Werror main.cpp -o app
```

Use it for:

- final quality checks
- continuous integration
- exercises where the goal is clean code

Be careful with it while exploring:

```text
Warnings from different compilers can vary.
Use -Werror when you are ready to make warnings a gate.
```

Beginner workflow:

```text
Learn with warnings on.
Fix warnings.
Use Werror as a final check.
```

---

## Debug Build Versus Release Build

Debug build:

```bash
g++ -std=c++20 -g -O0 -Wall -Wextra -Wpedantic main.cpp -o app_debug
```

Release build:

```bash
g++ -std=c++20 -O2 -DNDEBUG -Wall -Wextra -Wpedantic main.cpp -o app
```

| Build | Purpose |
|---|---|
| Debug | easier debugging, less optimization |
| Release | faster executable, assertions disabled by `NDEBUG` |

`-O0` means no optimization.

`-O2` means optimize for speed without going extreme.

`-DNDEBUG` disables standard `assert` checks.

---

## Multi-File Compile

Project:

```text
hello/
  main.cpp
  greet.cpp
  greet.hpp
```

`greet.hpp`:

```cpp
#pragma once

#include <string>

std::string make_greeting(const std::string& name);
```

`greet.cpp`:

```cpp
#include "greet.hpp"

std::string make_greeting(const std::string& name) {
    return "Hello, " + name;
}
```

`main.cpp`:

```cpp
#include <iostream>

#include "greet.hpp"

int main() {
    std::cout << make_greeting("Ada") << '\n';
}
```

Compile both `.cpp` files:

```bash
g++ -std=c++20 -Wall -Wextra -Wpedantic -g main.cpp greet.cpp -o hello
```

Important:

```text
Headers are included by .cpp files.
.cpp files are compiled.
The linker combines the compiled pieces.
```

---

## Header Discipline

Headers should usually contain:

- declarations
- class definitions
- inline/template code when needed
- include guards or `#pragma once`

Source files should usually contain:

- function bodies
- non-template implementation details
- includes needed by those bodies

Beginner rule:

```text
Put "what exists" in headers.
Put "how it works" in .cpp files.
```

---

## Mini Project: Build A Two-File Calculator

Files:

```text
calculator/
  main.cpp
  calculator.cpp
  calculator.hpp
```

`calculator.hpp`:

```cpp
#pragma once

int add(int left, int right);
int subtract(int left, int right);
```

`calculator.cpp`:

```cpp
#include "calculator.hpp"

int add(int left, int right) {
    return left + right;
}

int subtract(int left, int right) {
    return left - right;
}
```

`main.cpp`:

```cpp
#include <iostream>

#include "calculator.hpp"

int main() {
    std::cout << add(2, 3) << '\n';
    std::cout << subtract(10, 4) << '\n';
}
```

Build:

```bash
g++ -std=c++20 -Wall -Wextra -Wpedantic -g main.cpp calculator.cpp -o calculator
```

---

## Chapter Checkpoint

You should now be able to answer:

- What does `-std=c++20` do?
- Why should warnings be enabled?
- What does `-g` do?
- What is the difference between debug and release builds?
- Why do you compile `.cpp` files, not headers directly?
- What belongs in a header?
- What belongs in a `.cpp` file?

---

## Recap

- Build commands are readable once you know the flags.
- Use strict warnings while learning.
- Treat warnings as clues, not noise.
- Debug builds prioritize inspection.
- Release builds prioritize runtime performance.
- Multi-file C++ separates declarations from implementation.

## Try It Yourself

Take one class from Chapter 03 and split it into:

- `thing.hpp`
- `thing.cpp`
- `main.cpp`

Compile with strict warnings and fix every warning.

---

[**Next ->** Tests And Assertions In C++](./02-tests-and-assertions-in-cpp.md)  
[**<- Previous** Chapter Start](./README.md)
