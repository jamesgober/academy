# C++ Functions And Parameters Cheat Sheet

[Reference Index](./README.md) / [C++](../README.md)

Use this page when choosing parameter styles or designing a function signature.
For the guided lesson, see [Functions, Parameters, and Returns](../course/02-core-language-basics/02-functions-parameters-and-returns.md).

## Basic Function

```cpp
int add(int left, int right) {
    return left + right;
}
```

Parts:

```text
int          return type
add          function name
int left     first parameter
int right    second parameter
return       send result back
```

## Parameter Styles

| Style | Example | Use When |
|---|---|---|
| by value | `int level` | Cheap copy, no caller mutation needed. |
| non-const reference | `std::string& text` | Function must modify caller's object. |
| const reference | `const std::string& text` | Avoid copy and promise not to modify. |
| pointer | `int* out_value` | Nullable or output-style parameter semantics. |

## By Value

```cpp
void set_level(int level) {
    std::cout << level << '\n';
}
```

The function gets its own copy.

## Non-Const Reference

```cpp
void trim_marker(std::string& text) {
    text += " [checked]";
}
```

The function can change the caller's object. Use this only when mutation is the
point.

## Const Reference

```cpp
void print_name(const std::string& name) {
    std::cout << name << '\n';
}
```

Good for larger read-only values.

## Pointer

```cpp
bool try_parse_int(const char* raw, int* out_value) {
    if (raw == nullptr || out_value == nullptr) {
        return false;
    }

    // parsing would happen here
    *out_value = 0;
    return true;
}
```

Pointers can be null, so check them when null is possible.

## Return A Struct For Grouped Results

```cpp
struct ScoreSummary {
    int total;
    double average;
};

ScoreSummary summarize(const std::vector<int>& scores) {
    int total = 0;

    for (int score : scores) {
        total += score;
    }

    double average = scores.empty()
        ? 0.0
        : static_cast<double>(total) / scores.size();

    return ScoreSummary{total, average};
}
```

Use a struct when multiple results belong together.

## Overloads

C++ allows multiple functions with the same name when parameter lists differ:

```cpp
void print(int value);
void print(const std::string& value);
```

Overloads should differ by meaningful semantics, not tiny ambiguous differences.

## Design Prompts

- Does the function do one job?
- Can the name be a clear verb phrase?
- Are parameters in a natural order?
- Can invalid input be rejected early?
- Should this return a value instead of mutating an output parameter?
- Would a small struct clarify several related parameters?

## Risk Notices

- Non-const references hide mutation at the call site.
- Raw pointers can be null or dangling.
- Returning references to local variables is invalid.
- Too many overloads can make calls ambiguous.
- Passing large objects by value can be expensive.

---

[Reference Index](./README.md) / [C++](../README.md)
