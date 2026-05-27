# Memory Safety and RAII Checklist

## Ownership rules

- prefer stack objects by default
- prefer `std::unique_ptr` for dynamic ownership
- use `std::shared_ptr` only with clear shared lifetime need
- avoid raw owning pointers in APIs

## Leak/lifetime checks

- every allocation has clear owner
- no use-after-free paths
- no double delete patterns
- sanitizer run clean before release

---

[← Reference Index](./README.md) · [C++](../README.md)
