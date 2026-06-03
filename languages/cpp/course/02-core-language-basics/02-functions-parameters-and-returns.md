<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

[Home](../../../../README.md) / [C++](../../README.md) / [Chapter 02](./README.md)

---

# Functions, Parameters, And Returns

> A function gives a name to a piece of behavior. Good functions make programs
> easier to read, test, reuse, and debug.

**You will learn:**
- How to write functions
- How parameters work
- How return values work
- When to pass by value
- When to pass by reference
- When to pass by `const&`
- How to return grouped results
- How to keep functions small and honest

**Before this page, you should know:** [Types, Variables, And Strings](./01-types-variables-and-strings.md)

---

## Basic Function

```cpp
int add(int left, int right) {
    return left + right;
}
```

Read:

```text
add takes two ints.
add returns one int.
```

Use:

```cpp
int total = add(2, 3);
```

---

## Function Shape

```text
return_type function_name(parameter_list) {
    body
}
```

Example:

```cpp
double calculate_total(double price, int quantity) {
    return price * quantity;
}
```

---

## `void`

Use `void` when a function does not return a value.

```cpp
#include <iostream>
#include <string>

void print_error(const std::string& message) {
    std::cout << "Error: " << message << '\n';
}
```

Call:

```cpp
print_error("Invalid price");
```

---

## Pass By Value

```cpp
int clamp_score(int score) {
    if (score < 0) {
        return 0;
    }

    if (score > 100) {
        return 100;
    }

    return score;
}
```

`score` is a copy.

Changing it inside the function does not change the caller's variable.

Use value for:

- `int`
- `double`
- `bool`
- `char`
- small simple structs

---

## Pass By Reference To Modify

```cpp
void normalize_score(int& score) {
    if (score < 0) {
        score = 0;
    }

    if (score > 100) {
        score = 100;
    }
}
```

Use:

```cpp
int score = 140;
normalize_score(score);
std::cout << score << '\n'; // 100
```

`int&` means the function can modify the caller's variable.

Use non-const references only when mutation is the point.

---

## Pass By `const&`

```cpp
#include <iostream>
#include <string>

void print_name(const std::string& name) {
    std::cout << name << '\n';
}
```

`const std::string&` means:

```text
Borrow the string without copying it.
Promise not to change it.
```

Use `const&` for larger objects you only read.

---

## Pointer Parameters

Use a pointer when "no object" is a valid option.

```cpp
void maybe_print_name(const std::string* name) {
    if (name == nullptr) {
        std::cout << "No name\n";
        return;
    }

    std::cout << *name << '\n';
}
```

Use references for required objects.

Use pointers for optional objects.

---

## Return Grouped Results

If a function naturally returns multiple values, create a struct.

```cpp
struct ParseResult {
    bool ok = false;
    int value = 0;
};

ParseResult parse_positive_int(int input) {
    if (input <= 0) {
        return {.ok = false, .value = 0};
    }

    return {.ok = true, .value = input};
}
```

This is clearer than guessing what a magic value like `-1` means.

---

## Function Overloading

C++ allows multiple functions with the same name if their parameter lists differ.

```cpp
int area(int side) {
    return side * side;
}

int area(int width, int height) {
    return width * height;
}
```

Use overloads when the operation is conceptually the same.

Do not overload unrelated behavior just to reuse a name.

---

## Real Example: Order Total

```cpp
#include <iostream>
#include <string>

struct OrderLine {
    std::string sku;
    int quantity = 0;
    double unit_price = 0.0;
};

bool is_valid(const OrderLine& line) {
    return !line.sku.empty()
        && line.quantity > 0
        && line.unit_price >= 0.0;
}

double line_total(const OrderLine& line) {
    return line.quantity * line.unit_price;
}

void print_line(const OrderLine& line) {
    std::cout << line.sku << ": " << line_total(line) << '\n';
}

int main() {
    OrderLine line{
        .sku = "KB-100",
        .quantity = 2,
        .unit_price = 49.99
    };

    if (!is_valid(line)) {
        std::cout << "Invalid order line\n";
        return 1;
    }

    print_line(line);
}
```

Notice:

- `is_valid` answers a yes/no question
- `line_total` calculates a value
- `print_line` handles output
- each function has one clear job

---

## Common Mistakes

### Mistake 1: Function Does Too Much

If a function reads input, validates, calculates, saves files, and prints output,
split it.

### Mistake 2: Non-Const Reference Without Need

```cpp
void print(std::string& text);
```

If it does not modify:

```cpp
void print(const std::string& text);
```

### Mistake 3: Returning Reference To Local

Wrong:

```cpp
const std::string& make_name() {
    std::string name = "Ada";
    return name;
}
```

Return by value:

```cpp
std::string make_name() {
    return "Ada";
}
```

---

## Chapter Checkpoint

You should now be able to answer:

- What does a function return type mean?
- What does `void` mean?
- When should you pass by value?
- When should you pass by `T&`?
- When should you pass by `const T&`?
- When does a pointer parameter make sense?
- Why might a struct return be clearer than multiple output parameters?
- Why should functions have one clear job?

---

## Recap

- Functions package behavior.
- Value parameters copy.
- Non-const references allow mutation.
- `const&` borrows without mutation.
- Pointers are useful for optional objects.
- Structs can return grouped results.
- Small focused functions are easier to test.

## Try It Yourself

Write functions for a shopping cart line:

- `bool is_valid(const CartLine& line)`
- `double line_total(const CartLine& line)`
- `void print_line(const CartLine& line)`

Then call all three from `main`.

---

[**Next ->** Conditionals: If, Else-If, Ternary, Switch](./03-conditionals-if-else-if-ternary-switch.md)  
[**<- Previous** Types, Variables, And Strings](./01-types-variables-and-strings.md)
