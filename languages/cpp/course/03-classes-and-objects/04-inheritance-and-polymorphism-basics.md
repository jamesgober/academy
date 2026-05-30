<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

[Home](../../../../README.md) / [C++](../../README.md) / [Chapter 03](./README.md)

---

# Inheritance And Polymorphism Basics

> Inheritance is powerful, but it is not the default answer. In modern C++, reach
> for composition first, and use inheritance when you truly need an "is-a"
> relationship with shared behavior or runtime substitution.

**You will learn:**
- What inheritance means
- What base and derived classes are
- What virtual functions do
- Why virtual destructors matter
- What polymorphism means in plain language
- Why composition is often simpler
- When inheritance is a good fit

**Before this page, you should know:** [Methods, `const`, And `static`](./03-methods-const-and-static.md)

---

## The Simple Mental Model

Inheritance says:

```text
Derived is a kind of Base.
```

Examples that can make sense:

```text
Circle is a Shape.
SavingsAccount is an Account.
FileLogger is a Logger.
```

Examples that are suspicious:

```text
Engine is a Car.
Address is a User.
Keyboard is a Computer.
```

Those are usually "has-a" relationships, better modeled with composition.

```text
Car has an Engine.
User has an Address.
Computer has a Keyboard.
```

---

## Basic Inheritance

```cpp
#include <iostream>
#include <string>
#include <utility>

class Animal {
public:
    explicit Animal(std::string name) : name_(std::move(name)) {}

    void print_name() const {
        std::cout << name_ << '\n';
    }

private:
    std::string name_;
};

class Dog : public Animal {
public:
    explicit Dog(std::string name) : Animal(std::move(name)) {}
};
```

`Dog : public Animal` means:

```text
Dog publicly inherits from Animal.
A Dog can be used where an Animal is expected.
```

## Polymorphism

Polymorphism means:

```text
Different concrete types can be used through a common interface.
```

Example:

```cpp
#include <iostream>
#include <memory>
#include <string>
#include <utility>
#include <vector>

class Shape {
public:
    virtual ~Shape() = default;

    virtual double area() const = 0;
    virtual void print() const = 0;
};

class Rectangle : public Shape {
public:
    Rectangle(double width, double height)
        : width_(width), height_(height) {}

    double area() const override {
        return width_ * height_;
    }

    void print() const override {
        std::cout << "Rectangle area=" << area() << '\n';
    }

private:
    double width_;
    double height_;
};

class Circle : public Shape {
public:
    explicit Circle(double radius) : radius_(radius) {}

    double area() const override {
        return 3.14159 * radius_ * radius_;
    }

    void print() const override {
        std::cout << "Circle area=" << area() << '\n';
    }

private:
    double radius_;
};

int main() {
    std::vector<std::unique_ptr<Shape>> shapes;

    shapes.push_back(std::make_unique<Rectangle>(10.0, 5.0));
    shapes.push_back(std::make_unique<Circle>(3.0));

    for (const auto& shape : shapes) {
        shape->print();
    }
}
```

Visual model:

```text
vector<unique_ptr<Shape>>
  [0] -> Rectangle
  [1] -> Circle

loop calls shape->print()
  Rectangle uses Rectangle::print
  Circle uses Circle::print
```

That is runtime polymorphism.

---

## `virtual`

This says derived classes may provide their own behavior:

```cpp
virtual double area() const = 0;
```

The `= 0` means the function is pure virtual.

Plain language:

```text
Shape promises that shapes have an area,
but Shape itself does not know how to calculate it.
```

A class with at least one pure virtual function is abstract.

You cannot create a plain `Shape`:

```cpp
Shape shape; // error
```

You create concrete derived types like `Rectangle` or `Circle`.

---

## `override`

Use `override` when a derived class implements a virtual function.

```cpp
double area() const override {
    return width_ * height_;
}
```

Why?

The compiler checks your intent.

If you accidentally write the wrong signature, `override` catches it.

Wrong:

```cpp
double area() override; // missing const, compiler catches it
```

Beginner rule:

```text
Always write override when overriding a virtual function.
```

---

## Virtual Destructors

If a class is meant to be used polymorphically, give it a virtual destructor.

```cpp
class Shape {
public:
    virtual ~Shape() = default;
    virtual double area() const = 0;
};
```

Why?

Because derived objects may be destroyed through a base pointer.

```cpp
std::unique_ptr<Shape> shape = std::make_unique<Circle>(3.0);
```

When `shape` is destroyed, C++ must destroy the full `Circle`, not just the
`Shape` part.

---

## Prefer Composition First

Composition means one type contains another.

```cpp
#include <string>
#include <utility>

class Engine {
public:
    explicit Engine(std::string serial) : serial_(std::move(serial)) {}

private:
    std::string serial_;
};

class Car {
public:
    Car(std::string model, Engine engine)
        : model_(std::move(model)), engine_(std::move(engine)) {}

private:
    std::string model_;
    Engine engine_;
};
```

This says:

```text
Car has an Engine.
```

That is clearer than pretending `Car` is an `Engine`.

---

## When Inheritance Is A Good Fit

Inheritance is reasonable when:

- there is a true "is-a" relationship
- callers should use derived objects through a base interface
- virtual behavior is useful
- the base class has a stable, small interface

Examples:

- `Logger` with `ConsoleLogger` and `FileLogger`
- `Shape` with `Circle` and `Rectangle`
- `Command` with many command types

---

## Common Mistakes

### Mistake 1: Inheritance For Code Reuse Only

If your only goal is "I want to reuse some fields," composition may be cleaner.

### Mistake 2: Missing Virtual Destructor

If a base class has virtual functions, it usually needs a virtual destructor.

### Mistake 3: Deep Inheritance Trees

Many layers of inheritance become hard to understand.

Prefer small interfaces and composition.

---

## Mini Project: Logger Interface

Build this:

```cpp
#include <iostream>
#include <memory>
#include <string>
#include <utility>
#include <vector>

class Logger {
public:
    virtual ~Logger() = default;
    virtual void log(const std::string& message) = 0;
};

class ConsoleLogger : public Logger {
public:
    void log(const std::string& message) override {
        std::cout << "[console] " << message << '\n';
    }
};

class PrefixLogger : public Logger {
public:
    explicit PrefixLogger(std::string prefix)
        : prefix_(std::move(prefix)) {}

    void log(const std::string& message) override {
        std::cout << prefix_ << " " << message << '\n';
    }

private:
    std::string prefix_;
};

int main() {
    std::vector<std::unique_ptr<Logger>> loggers;

    loggers.push_back(std::make_unique<ConsoleLogger>());
    loggers.push_back(std::make_unique<PrefixLogger>("[app]"));

    for (const auto& logger : loggers) {
        logger->log("started");
    }
}
```

Explain:

- Why `Logger` has a virtual destructor
- Why `log` is virtual
- Why the vector stores `unique_ptr<Logger>`
- Which objects are actually created at runtime

---

## Chapter Checkpoint

You should now be able to answer:

- What does inheritance mean?
- What is a base class?
- What is a derived class?
- What does virtual dispatch do?
- What does `override` protect you from?
- Why do polymorphic base classes need virtual destructors?
- What is composition?
- Why is composition often simpler than inheritance?

---

## Recap

- Inheritance models "is-a."
- Composition models "has-a."
- Polymorphism lets different concrete types share an interface.
- `virtual` enables runtime dispatch.
- `override` asks the compiler to verify your override.
- Polymorphic base classes should have virtual destructors.
- Modern C++ should prefer composition unless inheritance is truly needed.

## Try It Yourself

Create a `NotificationSender` base class with:

- virtual destructor
- `virtual void send(const std::string& message) = 0`

Then create:

- `EmailSender`
- `SmsSender`

Store them in `std::vector<std::unique_ptr<NotificationSender>>` and send one
message through each.

---

[**Next ->** Chapter 03 Checkpoint](./05-chapter-03-checkpoint.md)  
[**<- Previous** Methods, `const`, And `static`](./03-methods-const-and-static.md)
