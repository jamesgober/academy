<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [C++](../../README.md) · [Chapter 04](./README.md)

</div>

---
# Avoiding Leaks and Lifetime Bugs

Common bug classes:
- memory leak
- use-after-free
- dangling pointer/reference
- double delete

Prevention patterns:
- prefer stack objects and RAII wrappers
- prefer smart pointers over raw owning pointers
- keep ownership explicit in APIs
---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Smart Pointers: unique_ptr, shared_ptr, weak_ptr](./03-smart-pointers-unique-shared-weak.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Chapter 04 Checkpoint →](./05-chapter-04-checkpoint.md) |

</div>