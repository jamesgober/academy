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

[← Constructors and Destructors](./02-constructors-and-destructors.md) · [Chapter 03](./README.md)
