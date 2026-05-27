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
# Methods, const, and static

- `const` methods promise not to mutate object state.
- `static` members belong to class-level state.

```cpp
class Counter {
public:
    int value() const { return value_; }
    static int created();
private:
    int value_ = 0;
};
```
---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Constructors and Destructors](./02-constructors-and-destructors.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Inheritance and Polymorphism Basics →](./04-inheritance-and-polymorphism-basics.md) |

</div>