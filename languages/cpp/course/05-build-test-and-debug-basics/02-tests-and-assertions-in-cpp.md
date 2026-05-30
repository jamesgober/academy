<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

[Home](../../../../README.md) / [C++](../../README.md) / [Chapter 05](./README.md)

---

# Tests And Assertions In C++

> A test checks behavior from the outside. An assertion checks an assumption from
> the inside. Both help you catch mistakes while the program is still small
> enough to understand.

**You will learn:**
- Why tests matter
- How to write tiny no-framework tests
- What `assert` does
- What `NDEBUG` changes
- How to test classes
- How to test edge cases
- When assertions are not enough

**Before this page, you should know:** [Compiler Flags And Build Discipline](./01-compiler-flags-and-build-discipline.md)

---

## Testing Mental Model

A test says:

```text
Given this input,
when I run this code,
then I expect this result.
```

Example:

```text
Given add(2, 3),
when the function runs,
then the result should be 5.
```

---

## The Smallest Possible Test

```cpp
#include <cassert>

int add(int left, int right) {
    return left + right;
}

void test_adds_two_numbers() {
    assert(add(2, 3) == 5);
}

int main() {
    test_adds_two_numbers();
}
```

If the assertion passes, the program exits normally.

If it fails, the program stops and prints diagnostic information.

---

## A Tiny Test Runner

```cpp
#include <cassert>
#include <iostream>
#include <string>

int add(int left, int right) {
    return left + right;
}

void test_adds_positive_numbers() {
    assert(add(2, 3) == 5);
}

void test_adds_negative_numbers() {
    assert(add(-2, -3) == -5);
}

int main() {
    test_adds_positive_numbers();
    test_adds_negative_numbers();

    std::cout << "all tests passed\n";
}
```

This is not a professional test framework, but it teaches the core habit:

```text
Make behavior executable.
```

---

## Testing A Class

```cpp
#include <cassert>
#include <string>
#include <utility>

class TodoItem {
public:
    explicit TodoItem(std::string title)
        : title_(std::move(title)) {}

    const std::string& title() const {
        return title_;
    }

    bool done() const {
        return done_;
    }

    void mark_done() {
        done_ = true;
    }

private:
    std::string title_;
    bool done_ = false;
};

void test_new_item_is_open() {
    TodoItem item("learn tests");
    assert(!item.done());
}

void test_mark_done_changes_state() {
    TodoItem item("learn tests");
    item.mark_done();
    assert(item.done());
}

int main() {
    test_new_item_is_open();
    test_mark_done_changes_state();
}
```

Good tests are small and named after behavior.

---

## Edge Cases

Do not only test the happy path.

If a function divides:

```cpp
bool safe_divide(int left, int right, int& result) {
    if (right == 0) {
        return false;
    }

    result = left / right;
    return true;
}
```

Test success and failure:

```cpp
void test_safe_divide_success() {
    int result = 0;
    bool ok = safe_divide(10, 2, result);

    assert(ok);
    assert(result == 5);
}

void test_safe_divide_rejects_zero() {
    int result = 123;
    bool ok = safe_divide(10, 0, result);

    assert(!ok);
    assert(result == 123);
}
```

The second test checks that failure does not modify the output value.

---

## Assertions Inside Code

`assert` checks a condition during debug-style runs.

```cpp
#include <cassert>
#include <vector>

int first_score(const std::vector<int>& scores) {
    assert(!scores.empty());
    return scores.front();
}
```

This says:

```text
This function expects a non-empty vector.
If that assumption is false during development, stop immediately.
```

Do not use `assert` for normal user input validation.

Wrong:

```cpp
assert(username != "");
```

If a user can cause it, handle it with normal code.

---

## `NDEBUG`

When compiled with `-DNDEBUG`, standard `assert` checks are disabled.

```bash
g++ -std=c++20 -DNDEBUG main.cpp -o app
```

That means this:

```cpp
assert(!scores.empty());
```

may not run in release builds.

Rule:

```text
Assertions are for programmer mistakes.
Runtime checks are for expected bad input or recoverable errors.
```

---

## Folder Shape For Simple Tests

```text
calculator/
  calculator.hpp
  calculator.cpp
  main.cpp
  calculator_tests.cpp
```

Build app:

```bash
g++ -std=c++20 -Wall -Wextra -Wpedantic main.cpp calculator.cpp -o calculator
```

Build tests:

```bash
g++ -std=c++20 -Wall -Wextra -Wpedantic calculator_tests.cpp calculator.cpp -o calculator_tests
./calculator_tests
```

The tests compile against the same implementation file.

---

## Mini Project: Test Inventory Rules

```cpp
#include <cassert>
#include <string>
#include <utility>
#include <vector>

class Inventory {
public:
    bool add(std::string name, int quantity) {
        if (name.empty() || quantity < 0) {
            return false;
        }

        items_.push_back(Item{std::move(name), quantity});
        return true;
    }

    int size() const {
        return static_cast<int>(items_.size());
    }

private:
    struct Item {
        std::string name;
        int quantity;
    };

    std::vector<Item> items_;
};

void test_add_valid_item() {
    Inventory inventory;

    bool ok = inventory.add("Keyboard", 3);

    assert(ok);
    assert(inventory.size() == 1);
}

void test_reject_empty_name() {
    Inventory inventory;

    bool ok = inventory.add("", 3);

    assert(!ok);
    assert(inventory.size() == 0);
}

void test_reject_negative_quantity() {
    Inventory inventory;

    bool ok = inventory.add("Keyboard", -1);

    assert(!ok);
    assert(inventory.size() == 0);
}

int main() {
    test_add_valid_item();
    test_reject_empty_name();
    test_reject_negative_quantity();
}
```

---

## Chapter Checkpoint

You should now be able to answer:

- What does a test verify?
- What does an assertion verify?
- Why should test names describe behavior?
- Why should you test edge cases?
- Why is `assert` not enough for user input validation?
- What happens when `NDEBUG` is defined?
- How can tests compile against the same `.cpp` implementation as the app?

---

## Recap

- Tests make behavior executable.
- Assertions catch broken programmer assumptions.
- `assert` can be disabled in release builds.
- User input needs real validation, not assert-only checks.
- Small no-framework tests are enough to learn the habit.
- Later, professional C++ projects often use frameworks like Catch2, doctest, or GoogleTest.

## Try It Yourself

Write tests for your Chapter 04 inventory project:

- adding an item increases count
- duplicate id is rejected
- selling an item decreases quantity
- selling a missing item returns false
- removing sold-out items removes only sold-out items

---

[**Next ->** Debugging, Errors, And Warning Navigation](./03-debugging-errors-and-warning-navigation.md)  
[**<- Previous** Compiler Flags And Build Discipline](./01-compiler-flags-and-build-discipline.md)
