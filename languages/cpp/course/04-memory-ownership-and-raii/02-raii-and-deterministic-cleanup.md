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

[← Stack, Heap, Pointers, and References](./01-stack-heap-pointers-and-references.md) · [Chapter 04](./README.md)
