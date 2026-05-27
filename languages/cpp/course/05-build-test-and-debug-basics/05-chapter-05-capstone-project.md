<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [C++](../../README.md) · [Chapter 05](./README.md)

</div>

---
# Chapter 05 Capstone Project

Build a small C++ project with classes, tests, and ownership-safe resources.

## Suggested structure

```text
inventory-app/
├── CMakeLists.txt
├── src/
│   ├── main.cpp
│   ├── inventory.cpp
│   └── inventory.hpp
└── tests/
    └── inventory_tests.cpp
```

## Required outcomes

- strict build clean
- test suite passes
- sanitizer run clean
- ownership decisions documented

## Suggested implementation focus

- `inventory.hpp` defines class interfaces and ownership boundaries
- `inventory.cpp` implements behavior and resource management
- `main.cpp` handles orchestration and user flow
- `inventory_tests.cpp` verifies core business behavior and edge cases

## Example class sketch

```cpp
class Inventory {
public:
    bool addItem(const std::string& name, int quantity);
    int quantityOf(const std::string& name) const;
private:
    std::vector<Item> items_;
};
```

## Expected output examples

- add known item: prints success message
- query missing item: prints not-found message
- invalid input: graceful validation message

## Reviewer checklist

- Are class responsibilities clear and separated?
- Are parameter choices (`value`, `const&`, `&`) justified?
- Are warnings and sanitizer reports clean?
- Is memory ownership explicit and consistent?

## Delivery checklist

1. Build with strict flags.
2. Run tests.
3. Run sanitizer build.
4. Document decisions and known limitations.
---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Sanitizers and Memory-Issue Triage](./04-sanitizers-and-memory-issue-triage.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Track Overview →](../../README.md) |

</div>