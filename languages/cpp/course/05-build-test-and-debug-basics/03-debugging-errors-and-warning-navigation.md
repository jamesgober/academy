<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

[Home](../../../../README.md) / [C++](../../README.md) / [Chapter 05](./README.md)

---

# Debugging, Errors, And Warning Navigation

> Debugging is not guessing harder. Debugging is reading what the tool is telling
> you, shrinking the problem, and checking your assumptions one at a time.

**You will learn:**
- How to read compiler errors
- Why the first error often matters most
- How to triage warnings
- How to shrink a failing example
- How to inspect values with print debugging
- How to build for debugger-friendly output
- How to avoid chasing symptoms before causes

**Before this page, you should know:** [Tests And Assertions In C++](./02-tests-and-assertions-in-cpp.md)

---

## Read Error Messages Like Addresses

Example:

```text
main.cpp:18:9: error: no matching function for call to 'print_user'
```

Break it apart:

| Part | Meaning |
|---|---|
| `main.cpp` | file |
| `18` | line |
| `9` | column |
| `error` | severity |
| message | what failed |

Start at the first real error.

Later errors may be caused by the first one.

---

## Example: Wrong Argument Type

Code:

```cpp
#include <iostream>
#include <string>

void print_user(const std::string& name) {
    std::cout << name << '\n';
}

int main() {
    print_user(42);
}
```

Likely error:

```text
error: invalid initialization of reference of type 'const std::string&'
       from expression of type 'int'
```

Translation:

```text
The function wants a string.
You gave it an int.
```

Fix:

```cpp
print_user("Ada");
```

---

## Example: Missing Include

Code:

```cpp
int main() {
    std::vector<int> values{1, 2, 3};
}
```

Likely error:

```text
error: 'vector' is not a member of 'std'
```

Translation:

```text
You used std::vector, but did not include the vector header.
```

Fix:

```cpp
#include <vector>
```

---

## Example: Const Mismatch

Code:

```cpp
class User {
public:
    std::string name() {
        return name_;
    }

private:
    std::string name_ = "Ada";
};

void print_user(const User& user) {
    std::cout << user.name() << '\n';
}
```

Problem:

```text
print_user has const User&.
It can only call const methods.
name() is not marked const.
```

Fix:

```cpp
std::string name() const {
    return name_;
}
```

---

## Warnings Deserve Attention

Example warning:

```text
warning: comparison of integer expressions of different signedness
```

Usually means:

```text
You compared signed int with unsigned std::size_t.
```

Code:

```cpp
for (int i = 0; i < values.size(); ++i) {
    std::cout << values[i] << '\n';
}
```

Fix when you need the index:

```cpp
for (std::size_t i = 0; i < values.size(); ++i) {
    std::cout << values[i] << '\n';
}
```

Fix when you do not need the index:

```cpp
for (int value : values) {
    std::cout << value << '\n';
}
```

---

## Shrink The Problem

When a file is huge, make a tiny reproduction.

Instead of debugging the whole app:

```text
app crashes when loading inventory, printing report, saving file, and exiting
```

shrink to:

```text
Can I create one Item?
Can I add one Item to Inventory?
Can I print one Inventory?
Can I save one Inventory?
```

Debugging gets easier when the failing surface is small.

---

## Print Debugging

Print values at important boundaries:

```cpp
std::cout << "quantity before sell: " << quantity_ << '\n';
```

Useful things to print:

- function entry
- parameter values
- branch decisions
- vector sizes
- pointer null/not-null state
- ownership transfers

Do not leave noisy debug prints in final code unless they are intentional logs.

---

## Debug-Friendly Build

Use:

```bash
g++ -std=c++20 -g -O0 -Wall -Wextra -Wpedantic main.cpp -o app_debug
```

Why:

| Flag | Reason |
|---|---|
| `-g` | includes debug information |
| `-O0` | avoids optimization rearranging code |
| warnings | catches suspicious code early |

For MSVC:

```powershell
cl /std:c++20 /W4 /EHsc /Zi /Od main.cpp
```

---

## A Practical Debugging Loop

1. Reproduce the problem.
2. Read the first error or failure.
3. Shrink the case.
4. Add a test if the behavior should be stable.
5. Inspect inputs and object state.
6. Fix the smallest cause.
7. Rebuild with warnings.
8. Rerun the test or command.

This loop keeps you from making five random changes and not knowing which one
helped.

---

## Common Error Translations

| Message fragment | Common meaning |
|---|---|
| `undefined reference` | declaration exists, linker cannot find implementation |
| `no matching function` | wrong arguments or missing overload |
| `not declared in this scope` | missing declaration, include, namespace, or typo |
| `discarding qualifiers` | const/non-const mismatch |
| `multiple definition` | implementation placed in header without `inline`, or compiled twice |
| `segmentation fault` | invalid memory access at runtime |

---

## Mini Project: Fix The Broken Program

Broken:

```cpp
#include <iostream>
#include <string>
#include <vector>

class User {
public:
    explicit User(std::string name) : name_(std::move(name)) {}

    std::string name() {
        return name_;
    }

private:
    std::string name_;
};

void print_users(const std::vector<User>& users) {
    for (int i = 0; i < users.size(); ++i) {
        std::cout << users[i].name() << '\n';
    }
}

int main() {
    std::vector<User> users;
    users.emplace_back("Ada");
    print_users(users);
}
```

Problems to find:

- missing include for `std::move`
- `name()` should be `const`
- loop index type warning

Fixed:

```cpp
#include <iostream>
#include <string>
#include <utility>
#include <vector>

class User {
public:
    explicit User(std::string name) : name_(std::move(name)) {}

    const std::string& name() const {
        return name_;
    }

private:
    std::string name_;
};

void print_users(const std::vector<User>& users) {
    for (const User& user : users) {
        std::cout << user.name() << '\n';
    }
}

int main() {
    std::vector<User> users;
    users.emplace_back("Ada");
    print_users(users);
}
```

---

## Chapter Checkpoint

You should now be able to answer:

- How do you read `file:line:column`?
- Why should you start with the first real error?
- What does `undefined reference` usually mean?
- What does a const mismatch mean?
- How do you shrink a debugging problem?
- What build flags help debugging?

---

## Recap

- Compiler messages are structured.
- The first real error often causes later noise.
- Warnings are useful clues.
- Shrinking a failing case makes bugs easier to see.
- Debug builds make inspection easier.
- A repeatable debugging loop beats random edits.

## Try It Yourself

Take a working program and intentionally introduce:

- one missing include
- one wrong argument type
- one const mismatch
- one signed/unsigned warning

Build after each change and write down how the compiler described the problem.

---

[**Next ->** Sanitizers And Memory-Issue Triage](./04-sanitizers-and-memory-issue-triage.md)  
[**<- Previous** Tests And Assertions In C++](./02-tests-and-assertions-in-cpp.md)
