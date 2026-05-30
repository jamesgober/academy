# Functions and Parameters Cheat Sheet

## Parameter styles

- value: `int x`
- reference: `T& x`
- const reference: `const T& x`
- pointer: `T* p`

## When to choose each

- value: cheap copy, no caller mutation required
- reference: caller mutation required
- const reference: avoid copy, no mutation
- pointer: nullable or optional input/output semantics

## Example signature set

```cpp
void setLevel(int level);
void normalize(std::string& text);
void print(const std::string& text);
bool tryParse(const char* raw, int* outValue);
```

## Function overloads

C++ supports same function name with different signatures.

## Return guidance

- return value directly for simple result
- return struct for grouped results
- use exceptions or status objects based on project policy

## Overload caution

Overloads should differ by meaningful parameter semantics, not by tiny ambiguous differences.

---

[Reference Index](./README.md)  /  [C++](../README.md)

