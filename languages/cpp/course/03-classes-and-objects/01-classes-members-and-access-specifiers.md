<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

[Home](../../../../README.md) / [C++](../../README.md) / [Chapter 03](./README.md)

---

# Classes, Members, And Access Specifiers

> A class is not just a bag of variables. A good C++ class protects a useful
> idea so the rest of the program cannot accidentally put it into an impossible
> state.

**You will learn:**
- What a class represents
- What member variables and member functions are
- Why C++ uses `public` and `private`
- How to protect invariants
- Why names often end with `_`
- How to build a small useful class instead of a toy shell

**Before this page, you should know:** [Functions, Parameters, And Returns](../02-core-language-basics/02-functions-parameters-and-returns.md)

---

## The Beginner Mental Model

A class bundles:

```text
data + behavior + rules
```

Example idea:

```text
BankAccount
  data:
    owner name
    balance

  behavior:
    deposit
    withdraw
    print summary

  rules:
    balance cannot go below zero
    deposit amount must be positive
```

The rules matter. Without rules, code anywhere in the program could break your
object.

---

## A Tiny Class

```cpp
#include <iostream>
#include <string>

class BankAccount {
public:
    void set_owner(std::string owner) {
        owner_ = owner;
    }

    void deposit(int cents) {
        if (cents > 0) {
            balance_cents_ += cents;
        }
    }

    void print() const {
        std::cout << owner_ << " has " << balance_cents_ << " cents\n";
    }

private:
    std::string owner_;
    int balance_cents_ = 0;
};

int main() {
    BankAccount account;
    account.set_owner("Ada");
    account.deposit(500);
    account.print();
}
```

Important parts:

- `class BankAccount` defines a new type.
- `public` members can be used from outside the class.
- `private` members can only be used by the class itself.
- `owner_` and `balance_cents_` are member variables.
- `deposit` and `print` are member functions.

---

## Why Private Data Matters

If `balance_cents_` were public, outside code could do this:

```cpp
account.balance_cents_ = -900000;
```

That breaks the account.

With private data, outside code must use methods:

```cpp
account.deposit(500);
```

The method can enforce the rule:

```cpp
if (cents > 0) {
    balance_cents_ += cents;
}
```

This is called protecting an invariant.

Plain language:

```text
An invariant is something that should always be true about an object.
```

For `BankAccount`:

```text
balance_cents_ should not be negative.
```

---

## Access Specifiers

| Specifier | Meaning |
|---|---|
| `public` | Code outside the class can use it |
| `private` | Only this class can use it |
| `protected` | This class and derived classes can use it |

Beginner rule:

```text
Put the interface in public.
Put the data in private.
Avoid protected until inheritance is actually needed.
```

Most beginner classes should look like:

```cpp
class Thing {
public:
    // functions other code should call

private:
    // data and helper functions
};
```

---

## Naming Member Variables

This course uses trailing underscores:

```cpp
std::string owner_;
int balance_cents_ = 0;
```

That is a common C++ style.

It helps you see:

```text
owner_ is part of the object.
owner is probably a parameter or local variable.
```

Example:

```cpp
void set_owner(std::string owner) {
    owner_ = owner;
}
```

---

## Public Interface Versus Private Implementation

Think of a class like a small machine:

```text
public buttons:
  deposit
  withdraw
  print

private gears:
  owner_
  balance_cents_
  helper calculations
```

Users of the class should not need to know every internal detail.

This gives you freedom to improve the internals later.

---

## A More Useful Version

```cpp
#include <iostream>
#include <string>
#include <utility>

class BankAccount {
public:
    explicit BankAccount(std::string owner)
        : owner_(std::move(owner)) {}

    void deposit(int cents) {
        if (cents <= 0) {
            return;
        }

        balance_cents_ += cents;
    }

    bool withdraw(int cents) {
        if (cents <= 0) {
            return false;
        }

        if (cents > balance_cents_) {
            return false;
        }

        balance_cents_ -= cents;
        return true;
    }

    int balance_cents() const {
        return balance_cents_;
    }

    void print() const {
        std::cout << owner_ << " has "
                  << balance_cents_ << " cents\n";
    }

private:
    std::string owner_;
    int balance_cents_ = 0;
};

int main() {
    BankAccount account("Ada");

    account.deposit(1200);

    if (!account.withdraw(1500)) {
        std::cout << "withdraw failed\n";
    }

    account.withdraw(200);
    account.print();
}
```

Notice:

- the constructor requires an owner
- bad deposits are ignored
- impossible withdrawals fail
- the balance can be read but not directly overwritten

---

## Why `explicit`?

This constructor:

```cpp
explicit BankAccount(std::string owner);
```

prevents surprising automatic conversions.

Without `explicit`, C++ may allow things like:

```cpp
BankAccount account = "Ada";
```

With `explicit`, the caller must be clear:

```cpp
BankAccount account("Ada");
```

Beginner rule:

```text
Use explicit on constructors that take one main argument.
```

---

## Class Versus Struct

In C++, `class` and `struct` are almost the same, except default access:

| Keyword | Default access |
|---|---|
| `class` | `private` |
| `struct` | `public` |

Common style:

- use `struct` for simple public data bundles
- use `class` when you need behavior and protected rules

Example `struct`:

```cpp
struct Point {
    int x = 0;
    int y = 0;
};
```

Example `class`:

```cpp
class BankAccount {
public:
    bool withdraw(int cents);

private:
    int balance_cents_ = 0;
};
```

---

## Common Mistakes

### Mistake 1: Public Data Everywhere

This makes it hard to protect rules:

```cpp
class Player {
public:
    int health = 100;
};
```

Better:

```cpp
class Player {
public:
    void take_damage(int amount) {
        if (amount > 0) {
            health_ -= amount;
            if (health_ < 0) {
                health_ = 0;
            }
        }
    }

    int health() const {
        return health_;
    }

private:
    int health_ = 100;
};
```

### Mistake 2: Getters And Setters With No Rules

If every private member has a public setter, you may not be protecting anything.

Ask:

```text
What invalid state am I preventing?
```

### Mistake 3: Classes That Do Too Much

A class named `ApplicationManagerControllerHelper` probably has too many jobs.

Prefer small classes with clear responsibility.

---

## Mini Project: Temperature Reading

Build a `Temperature` class:

- store Celsius internally
- construct from Celsius
- provide `celsius()` and `fahrenheit()` getters
- reject impossible values below `-273.15`

Starter:

```cpp
class Temperature {
public:
    explicit Temperature(double celsius) {
        if (celsius < -273.15) {
            celsius_ = -273.15;
        } else {
            celsius_ = celsius;
        }
    }

    double celsius() const {
        return celsius_;
    }

    double fahrenheit() const {
        return celsius_ * 9.0 / 5.0 + 32.0;
    }

private:
    double celsius_ = 0.0;
};
```

Explain the invariant:

```text
celsius_ is never below absolute zero.
```

---

## Chapter Checkpoint

You should now be able to answer:

- What does a class bundle together?
- What is a member variable?
- What is a member function?
- Why should data usually be private?
- What is an invariant?
- What does `public` mean?
- What does `private` mean?
- Why is `explicit` useful?
- When might you use `struct` instead of `class`?

---

## Recap

- Classes define new types.
- Good classes protect rules, not just data.
- Public members are the usable interface.
- Private members are implementation details.
- Invariants are truths your object should preserve.
- Prefer private data and public behavior.
- Use `explicit` to avoid surprising construction.

## Try It Yourself

Create a `TodoItem` class with:

- title
- done/not done state
- `mark_done()`
- `mark_open()`
- `is_done() const`
- `print() const`

Keep the data private.

---

[**Next ->** Constructors And Destructors](./02-constructors-and-destructors.md)  
[**<- Previous** Chapter Start](./README.md)
