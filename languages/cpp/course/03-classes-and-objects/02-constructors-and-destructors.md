<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

[Home](../../../../README.md) / [C++](../../README.md) / [Chapter 03](./README.md)

---

# Constructors And Destructors

> Constructors make sure an object starts life valid. Destructors give an object
> one final chance to clean up when its lifetime ends.

**You will learn:**
- What constructors do
- Why initialization lists matter
- What destructors do
- When you need a custom destructor
- Why most beginner classes should not manually own resources
- How construction and destruction order works

**Before this page, you should know:** [Classes, Members, And Access Specifiers](./01-classes-members-and-access-specifiers.md)

---

## Object Lifetime In Plain English

Every object has a lifetime:

```text
created -> used -> destroyed
```

Constructors run at the beginning.

Destructors run at the end.

Visual model:

```text
BankAccount account("Ada");
        |
        constructor runs
        |
        account is used
        |
        scope ends
        |
        destructor runs
```

---

## Constructors

A constructor has the same name as the class and no return type.

```cpp
#include <string>
#include <utility>

class User {
public:
    User(int id, std::string name)
        : id_(id), name_(std::move(name)) {}

private:
    int id_;
    std::string name_;
};
```

The part after `:` is the member initializer list.

```cpp
: id_(id), name_(std::move(name))
```

Read it as:

```text
initialize id_ with id
initialize name_ by moving from name
```

---

## Prefer Initialization Lists

This works:

```cpp
class User {
public:
    User(int id, std::string name) {
        id_ = id;
        name_ = std::move(name);
    }

private:
    int id_ = 0;
    std::string name_;
};
```

But this is better:

```cpp
class User {
public:
    User(int id, std::string name)
        : id_(id), name_(std::move(name)) {}

private:
    int id_;
    std::string name_;
};
```

Why?

The initializer list constructs members directly with the intended values.

Assignment in the constructor body means members were already constructed first,
then changed.

Beginner rule:

```text
Use member initializer lists for constructors.
```

---

## Default Constructors

A default constructor can be called with no arguments.

```cpp
class Counter {
public:
    Counter() = default;

    int value() const {
        return value_;
    }

private:
    int value_ = 0;
};
```

Because `value_` has an in-class initializer, the default constructor creates a
valid counter.

```cpp
Counter counter;
```

---

## Delete Construction You Do Not Want

Some objects should not be default-constructed.

```cpp
class ConnectionSettings {
public:
    ConnectionSettings() = delete;

    ConnectionSettings(std::string host, int port)
        : host_(std::move(host)), port_(port) {}

private:
    std::string host_;
    int port_;
};
```

Now this is not allowed:

```cpp
ConnectionSettings settings; // error
```

The caller must provide required information.

---

## Destructors

A destructor:

- has the class name with `~`
- has no return type
- takes no parameters
- runs when the object is destroyed

```cpp
#include <iostream>

class Tracer {
public:
    explicit Tracer(int id) : id_(id) {
        std::cout << "construct " << id_ << '\n';
    }

    ~Tracer() {
        std::cout << "destroy " << id_ << '\n';
    }

private:
    int id_;
};
```

Use:

```cpp
int main() {
    Tracer first(1);

    {
        Tracer second(2);
    }
}
```

Output:

```text
construct 1
construct 2
destroy 2
destroy 1
```

The inner object is destroyed first because its scope ends first.

---

## Destruction Order

For members, destruction happens in reverse construction order.

```cpp
class Team {
public:
    Team() = default;

private:
    Tracer first_{1};
    Tracer second_{2};
};
```

Construction:

```text
first_
second_
```

Destruction:

```text
second_
first_
```

This matters when one member depends on another.

---

## Most Classes Do Not Need Custom Destructors

This class does not need a destructor:

```cpp
#include <string>
#include <utility>
#include <vector>

class Playlist {
public:
    void add(std::string song) {
        songs_.push_back(std::move(song));
    }

private:
    std::vector<std::string> songs_;
};
```

`std::vector` and `std::string` clean up themselves.

Beginner rule:

```text
If all members clean themselves up, do not write a destructor.
```

This is part of a famous C++ guideline called the Rule of Zero.

---

## When A Destructor Is Needed

A custom destructor is needed when your class directly owns a resource that does
not clean itself up.

Example shape:

```cpp
class ManualResource {
public:
    ManualResource();
    ~ManualResource();

private:
    // raw handle from a C library
};
```

But while learning modern C++, prefer wrapping resources in existing RAII types:

- `std::vector`
- `std::string`
- `std::unique_ptr`
- `std::ifstream`
- `std::ofstream`
- `std::lock_guard`

You will study this deeply in [RAII And Deterministic Cleanup](../04-memory-ownership-and-raii/02-raii-and-deterministic-cleanup.md).

---

## Real Example: Timer Logging Scope

This example uses a destructor for visible behavior:

```cpp
#include <chrono>
#include <iostream>
#include <string>
#include <utility>

class ScopeTimer {
public:
    explicit ScopeTimer(std::string label)
        : label_(std::move(label)),
          started_(std::chrono::steady_clock::now()) {
        std::cout << "start " << label_ << '\n';
    }

    ~ScopeTimer() {
        auto ended = std::chrono::steady_clock::now();
        auto elapsed = std::chrono::duration_cast<std::chrono::milliseconds>(
            ended - started_
        );

        std::cout << "end " << label_ << " after "
                  << elapsed.count() << "ms\n";
    }

private:
    std::string label_;
    std::chrono::steady_clock::time_point started_;
};

void load_data() {
    ScopeTimer timer("load_data");

    for (int i = 0; i < 1'000'000; ++i) {
    }
}

int main() {
    load_data();
}
```

When `load_data` exits, the timer destructor runs.

This is the same lifetime idea used by file objects, locks, and smart pointers.

---

## Common Mistakes

### Mistake 1: Leaving Required Fields Uninitialized

Wrong:

```cpp
class User {
private:
    int id_;
};
```

Better:

```cpp
class User {
public:
    explicit User(int id) : id_(id) {}

private:
    int id_;
};
```

### Mistake 2: Doing Too Much Work In A Destructor

Destructors should release resources. They should not usually perform business
logic that can fail in complicated ways.

### Mistake 3: Manual Ownership Without Copy/Move Thinking

If a class owns a raw pointer and has a destructor, copying that class becomes
dangerous unless copy/move behavior is carefully designed.

Beginner fix:

```text
Do not own raw pointers. Use values, vectors, and smart pointers.
```

---

## Chapter Checkpoint

You should now be able to answer:

- What does a constructor do?
- What does a destructor do?
- Why are initializer lists preferred?
- What is a default constructor?
- When would you delete a default constructor?
- Why do most classes not need custom destructors?
- What does "Rule of Zero" mean at a beginner level?

---

## Recap

- Constructors create valid objects.
- Initializer lists initialize members directly.
- Destructors run when objects are destroyed.
- Scope controls destruction timing.
- Members are destroyed in reverse order.
- Prefer classes that do not need custom destructors.
- Use RAII types for resources.

## Try It Yourself

Create a `DownloadJob` class:

- constructor takes a URL string
- default construction is not allowed
- `print()` shows the URL
- no custom destructor needed

Then explain why the class is valid immediately after construction.

---

[**Next ->** Methods, `const`, And `static`](./03-methods-const-and-static.md)  
[**<- Previous** Classes, Members, And Access Specifiers](./01-classes-members-and-access-specifiers.md)
