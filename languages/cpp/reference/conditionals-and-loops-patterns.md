# Conditionals and Loops Patterns

## Conditionals

- `if`
- `if / else if / else`
- ternary: `cond ? a : b`
- `switch`

### Common forms

```cpp
if (ready) {
}

if (score >= 80) {
} else if (score >= 60) {
} else {
}

auto label = ok ? "yes" : "no";
```

Use ternary only for short value selection.

## Loops

- `for`
- `while`
- range-based `for`

### Common forms

```cpp
for (int index = 0; index < n; ++index) {
}

while (running) {
}

for (const auto& item : items) {
}
```

Use explicit names instead of default `i` when it improves readability.

---

[Reference Index](./README.md)  /  [C++](../README.md)

