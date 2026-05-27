<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [C++](../../README.md) · [Chapter 03](./README.md)

</div>

---
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

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Methods, const, and static](./03-methods-const-and-static.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Chapter 03 Checkpoint →](./05-chapter-03-checkpoint.md) |

</div>