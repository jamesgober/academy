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
# RAII and Deterministic Cleanup

RAII means resources are acquired in constructors and released in destructors.
This keeps cleanup deterministic and exception-safe.

```cpp
class LockGuard {
public:
    LockGuard(Mutex& m) : m_(m) { m_.lock(); }
    ~LockGuard() { m_.unlock(); }
private:
    Mutex& m_;
};
```
---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Stack, Heap, Pointers, and References](./01-stack-heap-pointers-and-references.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Smart Pointers: unique_ptr, shared_ptr, weak_ptr →](./03-smart-pointers-unique-shared-weak.md) |

</div>