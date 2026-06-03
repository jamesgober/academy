# C++ Errors, Warnings, And Sanitizers Guide

[Reference Index](./README.md) / [C++](../README.md)

Use this page when C++ compiler output or sanitizer reports are confusing. For
the course path, see [Reading Errors and Warnings](../course/01-getting-started/04-reading-errors-and-warnings.md) and [Sanitizers and Memory Issue Triage](../course/05-build-test-and-debug-basics/04-sanitizers-and-memory-issue-triage.md).

## Compiler Error Pattern

Example:

```text
main.cpp:18:9: error: no matching function for call to 'parse'
```

Read in order:

```text
file and line
severity
operation that failed
expected versus provided types/signature
```

Fix the first clear error, then rebuild. Later errors may be side effects.

## Warning Pattern

Example:

```text
main.cpp:42: warning: comparison of integer expressions of different signedness
```

Warnings often indicate future bugs even when an executable is produced.

Use strict warnings while learning:

```bash
g++ -std=c++20 -Wall -Wextra -Wpedantic -g main.cpp -o app
```

Use `-Werror` when you intentionally want warnings to fail the build:

```bash
g++ -std=c++20 -Wall -Wextra -Wpedantic -Werror -g main.cpp -o app
```

## Common Compiler Problems

Missing semicolon:

```cpp
std::cout << "hello\n"
```

Wrong name:

```cpp
std::cout << totla;
```

Wrong argument type:

```cpp
parse_number("abc", "not an int pointer");
```

Signed/unsigned comparison:

```cpp
for (int i = 0; i < values.size(); ++i) {
}
```

Prefer `std::size_t` for indexes that compare with `.size()`, or use a
range-based loop when no index is needed.

## AddressSanitizer Build

GCC or Clang:

```bash
g++ -std=c++20 -g -O1 -fsanitize=address -fno-omit-frame-pointer main.cpp -o app
./app
```

Clang:

```bash
clang++ -std=c++20 -g -O1 -fsanitize=address -fno-omit-frame-pointer main.cpp -o app
./app
```

AddressSanitizer helps catch:

- heap buffer overflow
- stack buffer overflow
- use after free
- double delete/free
- many leak cases

## UndefinedBehaviorSanitizer Build

```bash
g++ -std=c++20 -g -O1 -fsanitize=undefined main.cpp -o app
./app
```

This can catch issues such as signed integer overflow and invalid operations.

## Sanitizer Triage Flow

```text
1. Build with sanitizer flags.
2. Run the failing scenario.
3. Read the first reported bug type.
4. Find the stack frame in your code.
5. Identify ownership, bounds, or lifetime mistake.
6. Fix the root cause.
7. Rebuild and rerun.
```

## Memory Bug Vocabulary

| Bug | Meaning | Usual Fix Direction |
|---|---|---|
| use after free | object used after lifetime ended | fix ownership and lifetime |
| out of bounds | index/read/write past valid range | check size and loop bounds |
| double delete | same allocation released twice | make ownership single |
| leak | allocation never released | prefer RAII or add cleanup |

## Prefer RAII Fixes

When possible, prefer standard library ownership types:

```cpp
std::vector<int> values;
std::string name;
std::unique_ptr<Item> item;
```

Raw `new` and `delete` should be rare in beginner application code.

## Risk Notices

- Do not ignore warnings because the program "works."
- Do not patch sanitizer symptoms without fixing ownership or bounds.
- Do not return references or pointers to local variables.
- Do not manually manage memory when a standard container or smart pointer fits.
- Do not assume sanitizer-clean means bug-free; it means those tools did not
  catch a bug in that run.

---

[Reference Index](./README.md) / [C++](../README.md)
