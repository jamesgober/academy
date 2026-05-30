# Classes And OOP Cheat Sheet

[Reference Index](./README.md) / [C++](../README.md)

---

## Related Lessons

- [Classes, Members, And Access Specifiers](../course/03-classes-and-objects/01-classes-members-and-access-specifiers.md)
- [Constructors And Destructors](../course/03-classes-and-objects/02-constructors-and-destructors.md)
- [Methods, `const`, And `static`](../course/03-classes-and-objects/03-methods-const-and-static.md)
- [Inheritance And Polymorphism Basics](../course/03-classes-and-objects/04-inheritance-and-polymorphism-basics.md)
- [Chapter 03 Checkpoint](../course/03-classes-and-objects/05-chapter-03-checkpoint.md)

---

## Class Shape

```cpp
class BankAccount {
public:
    explicit BankAccount(std::string owner);

    void deposit(int cents);
    bool withdraw(int cents);
    int balance_cents() const;

private:
    std::string owner_;
    int balance_cents_ = 0;
};
```

Use this shape when:

- the type has rules to protect
- data should not be freely overwritten
- behavior belongs with the data

Notice:

```text
public = interface callers use
private = implementation details and protected state
```

---

## Access Specifiers

| Specifier | Use | Notice |
|---|---|---|
| `public` | methods callers should use | keep small and intentional |
| `private` | data and helper functions | default for member variables |
| `protected` | access for derived classes | avoid until inheritance is necessary |

Beginner default:

```text
public methods, private data
```

---

## Invariants

An invariant is a rule that should always be true.

Example:

```text
BankAccount balance is never negative.
Temperature is never below absolute zero.
TaskTitle is never empty.
```

Protect invariants by:

- requiring valid data in constructors
- validating method input
- keeping data private
- avoiding public setters that bypass rules

---

## Constructors

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

Use constructors to make objects valid immediately.

Use member initializer lists:

```cpp
: id_(id), name_(std::move(name))
```

Notice:

```text
Initializer lists construct members directly.
Assignment in the constructor body changes members after construction.
```

---

## `explicit`

```cpp
explicit User(std::string name);
```

Use `explicit` for constructors that take one main argument.

It prevents surprising implicit construction.

---

## Destructors

```cpp
class Tracer {
public:
    ~Tracer();
};
```

Destructors run when an object is destroyed.

Most beginner classes should not need custom destructors.

Prefer members that clean themselves up:

- `std::string`
- `std::vector`
- `std::unique_ptr`
- file stream objects
- lock guard objects

Notice:

```text
If all members manage themselves, follow the Rule of Zero and write no destructor.
```

---

## `const` Methods

```cpp
int size() const;
bool empty() const;
const std::string& name() const;
```

A `const` method promises not to mutate the object.

Use `const` when the method only reads state.

This allows:

```cpp
void print_user(const User& user) {
    std::cout << user.name() << '\n';
}
```

---

## `static`

Static member function:

```cpp
class Slug {
public:
    static std::string from_title(std::string title);
};
```

Call:

```cpp
auto slug = Slug::from_title("hello world");
```

Use static functions for behavior that belongs near a type but does not need one
object's state.

Notice:

```text
Mutable static data is shared state. Use carefully because it can make tests and
program behavior harder to reason about.
```

---

## Inheritance

```cpp
class Shape {
public:
    virtual ~Shape() = default;
    virtual double area() const = 0;
};

class Circle : public Shape {
public:
    double area() const override;
};
```

Use inheritance when:

- the derived type truly "is a" base type
- callers should use derived objects through a base interface
- runtime dispatch is needed

Use `override` on derived virtual functions.

Give polymorphic base classes virtual destructors.

---

## Composition

```cpp
class Engine {};

class Car {
private:
    Engine engine_;
};
```

Composition means:

```text
Car has an Engine.
```

Prefer composition for "has-a" relationships.

---

## Decision Table

| Need | Prefer |
|---|---|
| protect object rules | class with private data |
| simple public data bundle | struct |
| read-only method | `const` method |
| class-related helper with no object state | `static` member function |
| "has-a" relationship | composition |
| "is-a" runtime interface | inheritance with virtual functions |
| polymorphic base cleanup | virtual destructor |

---

## Risk Notices

- Public data makes invariants easy to break.
- Public setters for every field may not protect anything.
- Deep inheritance trees are hard to understand.
- Missing `override` can hide signature mistakes.
- Missing virtual destructors in polymorphic bases can cause incorrect cleanup.
- Custom destructors often imply copy/move design questions.

---

[Reference Index](./README.md) / [C++](../README.md)
