# Inheritance and Polymorphism Basics

Use inheritance when an "is-a" relationship is clear.
Use virtual functions for runtime polymorphic behavior.

```cpp
class Shape {
public:
    virtual ~Shape() = default;
    virtual double area() const = 0;
};
```

---

[← Methods, const, and static](./03-methods-const-and-static.md) · [Chapter 03](./README.md)
