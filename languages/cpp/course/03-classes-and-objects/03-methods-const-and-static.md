<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

[Home](../../../../README.md) / [C++](../../README.md) / [Chapter 03](./README.md)

---

# Methods, `const`, And `static`

> C++ lets you tell the compiler what kind of promise a method makes. `const`
> means "this method will not change the object." `static` means "this belongs
> to the class idea, not one particular object."

**You will learn:**
- What a method is
- What `this` means
- Why `const` methods matter
- How `const` helps readers and the compiler
- What `static` member functions are
- What `static` data members are
- When not to use `static`

**Before this page, you should know:** [Constructors And Destructors](./02-constructors-and-destructors.md)

---

## Methods Are Member Functions

A method is a function that belongs to a class.

```cpp
class Counter {
public:
    void increment() {
        value_++;
    }

    int value() const {
        return value_;
    }

private:
    int value_ = 0;
};
```

Use:

```cpp
Counter counter;
counter.increment();
std::cout << counter.value() << '\n';
```

The method runs on a specific object.

```text
counter.increment()
        |
        changes this counter object
```

---

## The Hidden `this` Pointer

Inside a non-static method, C++ gives you access to the current object through
`this`.

```cpp
class Counter {
public:
    void increment() {
        this->value_++;
    }

private:
    int value_ = 0;
};
```

You normally do not need to write `this->`, but knowing it exists helps.

Plain language:

```text
this means the object this method is currently running on.
```

---

## `const` Methods

This method promises not to change the object:

```cpp
int value() const {
    return value_;
}
```

The `const` after the parameter list is the important part:

```cpp
int value() const
            ^^^^^
```

It means:

```text
Calling this method will not mutate the object's normal state.
```

---

## Why `const` Matters

Suppose a function receives a read-only reference:

```cpp
void print_counter(const Counter& counter) {
    std::cout << counter.value() << '\n';
}
```

Because `counter` is `const`, C++ only allows calling `const` methods on it.

If `value()` were not marked `const`, this would fail:

```cpp
counter.value(); // only allowed if value() is const
```

That is good. It prevents accidental mutation through read-only code.

---

## Const And Non-Const Pairs

Sometimes you provide both versions.

```cpp
#include <string>
#include <vector>

class Notebook {
public:
    std::string& page_at(int index) {
        return pages_.at(index);
    }

    const std::string& page_at(int index) const {
        return pages_.at(index);
    }

private:
    std::vector<std::string> pages_{"first", "second"};
};
```

Use:

```cpp
Notebook notebook;
notebook.page_at(0) = "updated";

const Notebook read_only;
std::cout << read_only.page_at(0) << '\n';
```

Non-const objects can call the modifying version.

Const objects call the read-only version.

---

## `static` Member Functions

A static member function belongs to the class, not a specific object.

```cpp
#include <string>

class Slug {
public:
    static std::string from_title(std::string title) {
        for (char& ch : title) {
            if (ch == ' ') {
                ch = '-';
            }
        }

        return title;
    }
};
```

Use:

```cpp
std::string slug = Slug::from_title("hello world");
```

There is no `Slug` object.

That makes sense because the function does not need object state.

---

## `static` Data Members

A static data member is shared by all objects of the class.

```cpp
class Counter {
public:
    Counter() {
        created_++;
    }

    static int created() {
        return created_;
    }

private:
    inline static int created_ = 0;
};
```

Use:

```cpp
Counter first;
Counter second;

std::cout << Counter::created() << '\n'; // 2
```

Visual model:

```text
Counter first   \
Counter second   -> shared Counter::created_
Counter third   /
```

---

## Do Not Abuse `static`

`static` can become hidden global state.

Hidden global state can make code harder to test because one test may affect
another.

Use static member functions for:

- helpers that naturally belong with a type
- named constructors/factories
- simple counters or constants when appropriate

Avoid static state for:

- application-wide mutable settings
- test-sensitive counters
- things that should be passed as dependencies

---

## Real Example: Product Code

```cpp
#include <cctype>
#include <iostream>
#include <string>
#include <utility>

class ProductCode {
public:
    explicit ProductCode(std::string value)
        : value_(normalize(std::move(value))) {}

    const std::string& value() const {
        return value_;
    }

    bool empty() const {
        return value_.empty();
    }

    static bool is_valid_char(char ch) {
        return std::isalnum(static_cast<unsigned char>(ch)) || ch == '-';
    }

private:
    static std::string normalize(std::string input) {
        std::string output;

        for (char ch : input) {
            if (ch == ' ') {
                output.push_back('-');
            } else if (is_valid_char(ch)) {
                output.push_back(
                    static_cast<char>(std::toupper(static_cast<unsigned char>(ch)))
                );
            }
        }

        return output;
    }

    std::string value_;
};

int main() {
    ProductCode code("kb 100");
    std::cout << code.value() << '\n';
}
```

Notice:

- `value()` is `const` because it only reads
- `empty()` is `const` because it only reads
- `is_valid_char` is `static` because it does not need an object
- `normalize` is private because outside code does not need it

---

## Common Mistakes

### Mistake 1: Forgetting `const`

If a method only reads object state, mark it `const`.

```cpp
int size() const;
bool empty() const;
std::string name() const;
```

### Mistake 2: Returning Mutable References Accidentally

This exposes private state:

```cpp
std::string& name() {
    return name_;
}
```

Prefer:

```cpp
const std::string& name() const {
    return name_;
}
```

Return mutable references only when you truly want callers to mutate internals.

### Mistake 3: Making Everything Static

If a function needs object state, it should not be static.

---

## Chapter Checkpoint

You should now be able to answer:

- What is a method?
- What does `this` mean?
- What does a `const` method promise?
- Why can `const Counter&` only call `const` methods?
- What does a static member function belong to?
- What is static data shared by?
- Why can mutable static state make tests harder?

---

## Recap

- Methods are functions on objects.
- `this` points to the current object.
- `const` methods promise not to mutate normal object state.
- `const` makes APIs safer and clearer.
- Static member functions do not run on an object.
- Static data is shared by all objects of the class.
- Use `static` intentionally, not as a shortcut around design.

## Try It Yourself

Create a `Username` class:

- constructor takes a string
- `value() const` returns the stored username
- `empty() const` reports whether it is empty
- `static bool is_allowed_char(char ch)` checks letters, numbers, and `_`
- keep normalization helper private

---

[**Next ->** Inheritance And Polymorphism Basics](./04-inheritance-and-polymorphism-basics.md)  
[**<- Previous** Constructors And Destructors](./02-constructors-and-destructors.md)
