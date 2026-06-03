<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

[Home](../../../../README.md) / [C++](../../README.md) / [Chapter 02](./README.md)

---

# Conditionals: If, Else-If, Ternary, Switch

> Conditionals let your program choose different paths. Good conditionals make
> rules readable. Bad conditionals hide bugs inside tangled branches.

**You will learn:**
- How `if` works
- How `else if` and `else` work
- How guard clauses reduce nesting
- When the ternary operator is appropriate
- When `switch` is clearer
- How to avoid common branching mistakes

**Before this page, you should know:** [Functions, Parameters, And Returns](./02-functions-parameters-and-returns.md)

---

## Basic `if`

```cpp
if (score >= 60) {
    std::cout << "pass\n";
}
```

The condition must be something C++ can treat as true or false.

Common comparisons:

| Operator | Meaning |
|---|---|
| `==` | equal |
| `!=` | not equal |
| `<` | less than |
| `<=` | less than or equal |
| `>` | greater than |
| `>=` | greater than or equal |

---

## `else if` And `else`

```cpp
char grade_for(int score) {
    if (score >= 90) {
        return 'A';
    } else if (score >= 80) {
        return 'B';
    } else if (score >= 70) {
        return 'C';
    } else if (score >= 60) {
        return 'D';
    } else {
        return 'F';
    }
}
```

Only one branch runs.

The order matters.

If you check `score >= 60` first, almost every passing score would stop there.

---

## Guard Clauses

A guard clause handles an invalid or special case early.

```cpp
double price_after_discount(double price, double discount) {
    if (price < 0.0) {
        return 0.0;
    }

    if (discount < 0.0) {
        discount = 0.0;
    }

    if (discount > 1.0) {
        discount = 1.0;
    }

    return price * (1.0 - discount);
}
```

Guard clauses reduce deep nesting.

Instead of:

```cpp
if (user_exists) {
    if (password_ok) {
        if (account_active) {
            login();
        }
    }
}
```

prefer:

```cpp
if (!user_exists) {
    return;
}

if (!password_ok) {
    return;
}

if (!account_active) {
    return;
}

login();
```

---

## Logical Operators

| Operator | Meaning |
|---|---|
| `&&` | and |
| `||` | or |
| `!` | not |

Example:

```cpp
if (quantity > 0 && price >= 0.0) {
    std::cout << "valid line\n";
}
```

Example:

```cpp
if (role == "admin" || role == "owner") {
    std::cout << "full access\n";
}
```

---

## Ternary Operator

Use the ternary operator for short value selection.

```cpp
std::string status = score >= 60 ? "pass" : "fail";
```

Read:

```text
if score >= 60, use "pass"; otherwise use "fail"
```

Avoid complex ternary chains:

```cpp
auto label = a ? b : c ? d : e; // hard to read
```

Use `if` when logic needs room to breathe.

---

## `switch`

Use `switch` for one value with many discrete cases.

```cpp
std::string role_name(int role) {
    switch (role) {
    case 1:
        return "owner";
    case 2:
        return "editor";
    case 3:
        return "viewer";
    default:
        return "unknown";
    }
}
```

This is clearer than a long chain of `if (role == ...)`.

---

## Switch Fallthrough

In older style `switch`, forgetting `break` can cause fallthrough.

```cpp
switch (choice) {
case 1:
    std::cout << "one\n";
    break;
case 2:
    std::cout << "two\n";
    break;
default:
    std::cout << "unknown\n";
    break;
}
```

When returning from each case, `break` is not needed because `return` exits the
function.

---

## Real Example: Order Validation

```cpp
#include <iostream>
#include <string>

struct OrderLine {
    std::string sku;
    int quantity = 0;
    double unit_price = 0.0;
};

std::string validation_message(const OrderLine& line) {
    if (line.sku.empty()) {
        return "SKU is required";
    }

    if (line.quantity <= 0) {
        return "Quantity must be positive";
    }

    if (line.unit_price < 0.0) {
        return "Unit price cannot be negative";
    }

    return "ok";
}

int main() {
    OrderLine line{
        .sku = "KB-100",
        .quantity = 2,
        .unit_price = 49.99
    };

    std::string message = validation_message(line);

    if (message != "ok") {
        std::cout << message << '\n';
        return 1;
    }

    std::cout << "line accepted\n";
}
```

This is better than nesting all checks inside one huge branch.

---

## Common Mistakes

### Mistake 1: Assignment Instead Of Comparison

Wrong:

```cpp
if (score = 100) {
}
```

Correct:

```cpp
if (score == 100) {
}
```

### Mistake 2: Deep Nesting

If your code is five levels deep, consider guard clauses or helper functions.

### Mistake 3: Ternary For Complex Logic

Ternary is for small value choices, not full business logic.

---

## Chapter Checkpoint

You should now be able to answer:

- What does `if` do?
- Why does branch order matter?
- What is a guard clause?
- What do `&&`, `||`, and `!` mean?
- When is ternary appropriate?
- When is `switch` clearer than `if`?
- Why should `default` usually exist?

---

## Recap

- Conditionals choose paths.
- `else if` chains should be ordered carefully.
- Guard clauses make invalid cases explicit.
- Ternary is good for short value selection.
- `switch` is good for discrete cases.
- Readability matters more than clever branching.

## Try It Yourself

Write a function:

```cpp
std::string account_status(int balance, bool locked);
```

Rules:

- locked accounts return `"locked"`
- negative balances return `"overdrawn"`
- zero balances return `"empty"`
- otherwise return `"active"`

---

[**Next ->** Loops And Iteration Patterns](./04-loops-and-iteration-patterns.md)  
[**<- Previous** Functions, Parameters, And Returns](./02-functions-parameters-and-returns.md)
