# OOP and Type Design Checklist

## Class design

- Keep state private by default.
- Validate at object boundaries.
- Keep each class focused on one responsibility.

## Interface design

- Depend on interfaces, not concrete implementations.
- Keep contracts minimal and cohesive.

## Type selection

- `class`: rich behavior and mutable domain entities
- `record`: immutable data-centric models with value equality
- `struct`: small immutable value types

## Constructor rules

- Make invalid state unrepresentable.
- Prefer explicit constructor arguments over hidden defaults.

---

[← C# Reference](./README.md)
