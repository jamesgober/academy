<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

[Home](../../../../README.md) / [C++](../../README.md) / [Chapter 04](./README.md)

---

# Avoiding Leaks And Lifetime Bugs

> C++ memory bugs usually come from one broken promise: code used an object after
> its lifetime ended, forgot to release a resource, or released it more than once.
> The fix is not memorizing scary words. The fix is learning to make lifetime
> visible in your design.

**You will learn:**
- What memory leaks are
- What dangling pointers and references are
- What use-after-free means
- Why double delete is dangerous
- How RAII prevents many bugs automatically
- How sanitizers help you catch mistakes
- How to smell risky ownership APIs before they hurt you

**Before this page, you should know:** [Smart Pointers](./03-smart-pointers-unique-shared-weak.md)

---

## The Four Big Lifetime Bugs

| Bug | Plain-language meaning |
|---|---|
| Memory leak | You allocated something and lost the cleanup path |
| Dangling pointer/reference | You still refer to an object that no longer exists |
| Use-after-free | You use memory after it has been released |
| Double delete | You release the same allocation twice |

All four are easier to understand if you track ownership:

```text
Who owns this object?
When does the owner destroy it?
Can anyone still refer to it after that?
```

---

## Memory Leak

Leak:

```cpp
void bad() {
    int* values = new int[100];
    values[0] = 42;
} // delete[] never happens
```

The program allocated memory and then lost the only pointer to it.

Better:

```cpp
#include <vector>

void good() {
    std::vector<int> values(100);
    values[0] = 42;
}
```

`std::vector` owns the dynamic storage and releases it automatically.

Beginner translation:

```text
If your code says new[], first ask why this is not a vector.
```

---

## Leak Through Early Return

This is one reason manual cleanup is fragile:

```cpp
bool save_scores(bool can_save) {
    int* scores = new int[10];

    if (!can_save) {
        return false; // leak
    }

    delete[] scores;
    return true;
}
```

Better:

```cpp
#include <vector>

bool save_scores(bool can_save) {
    std::vector<int> scores(10);

    if (!can_save) {
        return false;
    }

    return true;
}
```

The vector cleans itself on every exit path.

---

## Dangling Pointer

A dangling pointer points to an object whose lifetime has ended.

```cpp
#include <iostream>

int* make_bad_pointer() {
    int value = 42;
    return &value;
}

int main() {
    int* pointer = make_bad_pointer();
    std::cout << *pointer << '\n'; // wrong
}
```

Visual model:

```text
make_bad_pointer starts
  value exists
  return address of value
make_bad_pointer ends
  value is destroyed

main receives address
  address points to dead object
```

Correct version:

```cpp
int make_value() {
    return 42;
}
```

Return the value, not a pointer to a local.

---

## Dangling Reference

References can dangle too.

Wrong:

```cpp
#include <string>
#include <utility>

const std::string& title() {
    std::string local = "Report";
    return local;
}
```

Good:

```cpp
#include <string>
#include <utility>

std::string title() {
    return "Report";
}
```

Rule:

```text
Do not return references or pointers to local variables.
```

---

## Use-After-Free

Manual version:

```cpp
#include <iostream>

int main() {
    int* value = new int(10);
    delete value;

    std::cout << *value << '\n'; // use-after-free
}
```

The pointer still stores an address, but the object at that address is no longer
yours to use.

Better:

```cpp
#include <memory>

int main() {
    auto value = std::make_unique<int>(10);
    // use value while it is alive
}
```

With a smart pointer, you usually do not manually release the object early.

If you do reset it:

```cpp
value.reset();
```

then treat it as empty:

```cpp
if (value == nullptr) {
    // no object
}
```

---

## Double Delete

Wrong:

```cpp
int* value = new int(10);

delete value;
delete value; // wrong
```

Deleting the same allocation twice corrupts your program's ownership story.

Better:

```cpp
auto value = std::make_unique<int>(10);
```

The owner destroys the object exactly once.

---

## The Ownership Smell Checklist

Be suspicious when you see:

- `new` and `delete` far apart
- a function returning a raw pointer
- a class with a raw pointer data member
- a class with a destructor but no clear copy/move story
- a function storing the result of `.get()`
- a pointer used after its owner moved or left scope
- `shared_ptr` used because ownership was not thought through

Not all of these are automatically wrong. They are places to slow down.

---

## Safer Replacements

| Risky pattern | Safer beginner pattern |
|---|---|
| `new T` | `std::make_unique<T>()` |
| `new T[n]` | `std::vector<T>` |
| manual file close | `std::ifstream` / `std::ofstream` |
| manual lock/unlock | `std::lock_guard<std::mutex>` |
| optional borrowed object | `T*` with null check |
| required borrowed object | `T&` or `const T&` |
| shared back-reference | `std::weak_ptr<T>` |

---

## Real Example: Fix A Raw Pointer Class

Risky version:

```cpp
#include <string>

class Player {
public:
    Player(std::string name, int score)
        : name_(std::move(name)), score_(new int(score)) {}

    ~Player() {
        delete score_;
    }

    int score() const {
        return *score_;
    }

private:
    std::string name_;
    int* score_;
};
```

This class owns `score_`, but the ownership is fragile.

Problem:

```cpp
Player a("Ada", 10);
Player b = a; // default copy copies the pointer
```

Now both objects point to the same `int`. Both destructors will try to delete it.

Better version:

```cpp
#include <string>

class Player {
public:
    Player(std::string name, int score)
        : name_(std::move(name)), score_(score) {}

    int score() const {
        return score_;
    }

private:
    std::string name_;
    int score_;
};
```

Best fix: do not allocate when a normal member works.

If dynamic ownership is truly needed:

```cpp
#include <memory>
#include <string>
#include <utility>

class Player {
public:
    Player(std::string name, int score)
        : name_(std::move(name)),
          score_(std::make_unique<int>(score)) {}

    int score() const {
        return *score_;
    }

private:
    std::string name_;
    std::unique_ptr<int> score_;
};
```

But for a single `int`, a plain member is better.

That is an important lesson:

> Smart pointers are not a prize. The simplest correct ownership model wins.

---

## Compile With Warnings

Use strong warnings while learning.

GCC or Clang:

```bash
g++ -std=c++20 -Wall -Wextra -Wpedantic main.cpp -o app
```

MSVC:

```powershell
cl /std:c++20 /W4 /EHsc main.cpp
```

Warnings often catch suspicious lifetime and type issues before runtime.

---

## Use Sanitizers

AddressSanitizer can catch many memory errors.

GCC or Clang:

```bash
g++ -std=c++20 -g -fsanitize=address,undefined -fno-omit-frame-pointer main.cpp -o app
./app
```

Sanitizers can help catch:

- use-after-free
- buffer overflows
- double delete
- some undefined behavior

They do not prove your program is perfect, but they are excellent training
wheels and debugging tools.

Note for Windows:

```text
Sanitizer support depends on compiler and version. Clang and recent MSVC toolsets
have support for AddressSanitizer, but exact commands vary.
```

---

## A Simple Debugging Routine

When a C++ program crashes around memory:

1. Rebuild with warnings.
2. Rebuild with debug symbols.
3. Run with sanitizers if available.
4. Find the first bad access, not the last symptom.
5. Ask who owns the object being accessed.
6. Ask whether the owner moved, reset, or left scope.

Memory errors often explode far away from the real mistake. Follow the lifetime.

---

## Mini Project: Leak-Proof A Scores Program

Start with this intentionally risky code:

```cpp
#include <iostream>

int* make_scores(int count) {
    int* scores = new int[count];

    for (int i = 0; i < count; ++i) {
        scores[i] = i * 10;
    }

    return scores;
}

void print_scores(int* scores, int count) {
    for (int i = 0; i < count; ++i) {
        std::cout << scores[i] << '\n';
    }
}

int main() {
    int count = 5;
    int* scores = make_scores(count);
    print_scores(scores, count);
    delete[] scores;
}
```

Refactor it to:

```cpp
#include <iostream>
#include <vector>

std::vector<int> make_scores(int count) {
    std::vector<int> scores;
    scores.reserve(count);

    for (int i = 0; i < count; ++i) {
        scores.push_back(i * 10);
    }

    return scores;
}

void print_scores(const std::vector<int>& scores) {
    for (int score : scores) {
        std::cout << score << '\n';
    }
}

int main() {
    std::vector<int> scores = make_scores(5);
    print_scores(scores);
}
```

Then explain:

- Who owns the scores?
- When are they destroyed?
- Why is there no `delete[]`?
- Why does `print_scores` take `const std::vector<int>&`?

---

## Chapter Checkpoint

You should now be able to answer:

- What is a memory leak?
- What is a dangling pointer?
- Why can references dangle?
- What is use-after-free?
- Why is double delete dangerous?
- Why does `std::vector` prevent many dynamic-array mistakes?
- What warning signs suggest risky ownership?
- What do sanitizers help catch?

---

## Recap

- Lifetime bugs happen when ownership and access disagree.
- Leaks mean cleanup was missed.
- Dangling pointers and references refer to dead objects.
- Use-after-free means using released memory.
- Double delete means releasing the same allocation twice.
- Prefer values, containers, and RAII owners.
- Use warnings and sanitizers early while learning.

## Try It Yourself

Find one old-style C++ example online or in a book that uses `new` and `delete`.
Rewrite it using values, `std::vector`, or `std::unique_ptr`. Then write a short
ownership paragraph explaining why the new version is safer.

---

[**Next ->** Chapter 04 Checkpoint](./05-chapter-04-checkpoint.md)  
[**<- Previous** Smart Pointers: `unique_ptr`, `shared_ptr`, `weak_ptr`](./03-smart-pointers-unique-shared-weak.md)
