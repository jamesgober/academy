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

[← Smart Pointers: unique_ptr, shared_ptr, weak_ptr](./03-smart-pointers-unique-shared-weak.md) · [Chapter 04](./README.md)
