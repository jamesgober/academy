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

[← Classes, Members, and Access Specifiers](./01-classes-members-and-access-specifiers.md) · [Chapter 03](./README.md)
