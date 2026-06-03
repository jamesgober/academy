# C++ Conditionals And Loops Patterns

[Reference Index](./README.md) / [C++](../README.md)

Use this page when you need to choose a decision or repetition pattern. For the
lesson version, see [Conditionals](../course/02-core-language-basics/03-conditionals-if-else-if-ternary-switch.md) and [Loops](../course/02-core-language-basics/04-loops-and-iteration-patterns.md).

## `if`

```cpp
if (ready) {
    std::cout << "ready\n";
}
```

Use `if` when code should run only under a condition.

## `if` / `else`

```cpp
if (score >= 60) {
    std::cout << "pass\n";
} else {
    std::cout << "needs practice\n";
}
```

Use this for a clear two-path choice.

## `else if`

```cpp
if (score >= 90) {
    grade = 'A';
} else if (score >= 80) {
    grade = 'B';
} else if (score >= 70) {
    grade = 'C';
} else {
    grade = 'F';
}
```

Order matters. The first true branch runs.

## Ternary Operator

```cpp
auto label = ready ? "ready" : "not ready";
```

Use ternary for short value selection. Avoid it for complex logic.

## `switch`

```cpp
switch (menu_choice) {
case 1:
    std::cout << "start\n";
    break;
case 2:
    std::cout << "settings\n";
    break;
default:
    std::cout << "unknown choice\n";
    break;
}
```

In C++, `break` matters. Without it, execution can continue into the next case.

## Counting `for`

```cpp
for (int index = 0; index < count; ++index) {
    std::cout << index << '\n';
}
```

Use when you need an index or a specific number of repetitions.

## Range-Based `for`

```cpp
for (const auto& item : items) {
    std::cout << item << '\n';
}
```

Use when you want each item and do not need the index.

`const auto&` avoids copying and promises not to change the item.

## `while`

```cpp
while (running) {
    process_next_command();
}
```

Use when repetition depends on a condition rather than a fixed count.

## Accumulation Pattern

```cpp
int total = 0;

for (int score : scores) {
    total += score;
}
```

Start with a neutral value, update it during the loop, then use the result.

## Search Pattern

```cpp
bool found = false;

for (const auto& name : names) {
    if (name == target) {
        found = true;
        break;
    }
}
```

Use `break` when continuing cannot change the answer.

## Risk Notices

- Do not use `=` when you mean `==`.
- Do not omit `break` in `switch` unless fallthrough is intentional and clearly
  documented.
- Avoid deeply nested branches when guard clauses would be clearer.
- Do not use unsigned indexes casually with negative comparisons.
- Prefer range-based loops when indexes are not needed.

---

[Reference Index](./README.md) / [C++](../README.md)
