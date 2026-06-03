<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

[Home](../../../../README.md) / [C++](../../README.md) / [Chapter 02](./README.md)

---

# Types, Variables, And Strings

> A variable is a named piece of data. A type tells C++ what kind of data it is,
> what operations make sense, and how much memory it usually needs.

**You will learn:**
- What variables are
- Common C++ types
- Why initialization matters
- How `auto` should be used
- Why `std::string` is the beginner text type
- How numeric conversion can surprise you
- How to read simple input safely

**Before this page, you should know:** [Your First C++ Program](../01-getting-started/02-your-first-cpp-program.md)

---

## Variables

```cpp
int score = 100;
double temperature = 72.5;
bool is_online = true;
std::string name = "Ada";
```

Read:

```text
score is an int.
temperature is a double.
is_online is a bool.
name is a std::string.
```

Variables should start with a useful value.

Prefer:

```cpp
int count = 0;
```

Avoid:

```cpp
int count;
```

An uninitialized local variable can contain an unpredictable value.

---

## Common Types

| Type | Use | Example |
|---|---|---|
| `int` | whole numbers | `42` |
| `long long` | larger whole numbers | `9000000000LL` |
| `double` | decimal/floating-point values | `3.14` |
| `bool` | true/false | `true` |
| `char` | one character | `'A'` |
| `std::string` | text | `"hello"` |

Include string:

```cpp
#include <string>
```

---

## Fixed-Width Integers

When exact size matters, use fixed-width integer types.

```cpp
#include <cstdint>

std::int32_t user_id = 1001;
std::int64_t file_size = 9000000000;
```

Use them when:

- reading binary formats
- network protocols specify sizes
- file formats specify sizes
- cross-platform size consistency matters

For normal beginner counters, `int` is fine.

---

## `auto`

`auto` asks C++ to infer the type.

```cpp
auto name = std::string{"Ada"};
auto score = 100;
```

Use `auto` when the type is obvious from the right side.

Good:

```cpp
auto total = price * quantity;
```

Less helpful for beginners:

```cpp
auto value = get_result();
```

if you cannot tell what `get_result()` returns.

Beginner rule:

```text
Use explicit types while learning unless auto clearly improves readability.
```

---

## Strings

```cpp
#include <iostream>
#include <string>

int main() {
    std::string first = "Ada";
    std::string last = "Lovelace";
    std::string full = first + " " + last;

    std::cout << full << '\n';
}
```

`std::string` handles text storage for you.

Unlike C-style character arrays, beginner C++ code should usually use
`std::string`.

---

## String Operations

```cpp
std::string name = "Ada";

std::cout << name.size() << '\n';
std::cout << name.empty() << '\n';
std::cout << name[0] << '\n';
```

Common methods:

| Method | Meaning |
|---|---|
| `.size()` | number of characters/bytes in the string representation |
| `.empty()` | true if size is zero |
| `.find("x")` | find substring position |
| `.substr(start, count)` | copy part of string |
| `.push_back(ch)` | append one character |

Notice:

```text
std::string indexing does not check bounds.
Use .at(index) when you want bounds checking while learning.
```

---

## Input With `std::cin`

```cpp
#include <iostream>
#include <string>

int main() {
    std::string name;

    std::cout << "Name: ";
    std::cin >> name;

    std::cout << "Hello, " << name << '\n';
}
```

`std::cin >> name` reads one word.

For a full line:

```cpp
std::string line;
std::getline(std::cin, line);
```

---

## Input Failure

```cpp
int age = 0;

std::cout << "Age: ";
std::cin >> age;

if (!std::cin) {
    std::cout << "That was not a valid number.\n";
    return 1;
}
```

Input can fail.

Real programs should not assume users type perfect values.

---

## Numeric Surprises

Integer division:

```cpp
std::cout << 5 / 2 << '\n'; // 2
```

Floating-point division:

```cpp
std::cout << 5.0 / 2.0 << '\n'; // 2.5
```

Overflow:

```cpp
int value = 2'000'000'000;
value = value + 2'000'000'000; // not safe
```

Use types with enough range and validate inputs.

---

## Real Example: Price Calculator

```cpp
#include <iostream>
#include <string>

int main() {
    std::string item;
    double price = 0.0;
    int quantity = 0;

    std::cout << "Item name: ";
    std::getline(std::cin, item);

    std::cout << "Price: ";
    std::cin >> price;

    if (!std::cin || price < 0.0) {
        std::cout << "Invalid price.\n";
        return 1;
    }

    std::cout << "Quantity: ";
    std::cin >> quantity;

    if (!std::cin || quantity < 0) {
        std::cout << "Invalid quantity.\n";
        return 1;
    }

    double total = price * quantity;
    std::cout << item << " total: " << total << '\n';
}
```

This is still small, but it is real:

- text input
- numeric input
- validation
- calculation
- output

---

## Chapter Checkpoint

You should now be able to answer:

- What is a variable?
- What is a type?
- Why should local variables be initialized?
- When is `std::string` better than C-style text?
- What does `auto` do?
- What is integer division?
- How can input fail?
- Why might fixed-width integers matter?

---

## Recap

- Types describe what data can do.
- Initialize variables before using them.
- Use `std::string` for beginner text handling.
- Use `auto` when the inferred type is obvious.
- Validate user input.
- Integer math and floating-point math behave differently.

## Try It Yourself

Write a program that asks for:

- product name
- unit price
- quantity

Then prints the total or a friendly error if the input is invalid.

---

[**Next ->** Functions, Parameters, And Returns](./02-functions-parameters-and-returns.md)  
[**<- Previous** Chapter Start](./README.md)
