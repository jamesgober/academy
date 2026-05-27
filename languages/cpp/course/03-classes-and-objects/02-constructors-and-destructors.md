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
# Constructors and Destructors

Constructors initialize object state.
Destructors run cleanup when object lifetime ends.

```cpp
class FileHandle {
public:
    FileHandle();
    ~FileHandle();
};
```

Destructors are a key part of RAII style.
---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Classes, Members, and Access Specifiers](./01-classes-members-and-access-specifiers.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Methods, const, and static →](./03-methods-const-and-static.md) |

</div>