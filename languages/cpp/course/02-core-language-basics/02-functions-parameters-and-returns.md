# Functions, Parameters, and Returns

Functions package behavior into reusable units with explicit input/output design.

## Basic function

```cpp
int add(int a, int b) {
    return a + b;
}
```

## Parameter patterns

- pass by value: `int x`
- pass by reference: `int& x`
- pass by const reference: `const std::string& s`

## When to use each parameter style

- **value**: small cheap-to-copy types (`int`, `double`, small structs)
- **reference (`T&`)**: when function must modify caller object
- **const reference (`const T&`)**: avoid copies for larger types while preventing modification
- **pointer (`T*`)**: nullable or optional object, or C-style API interop

## Example with const reference and mutable reference

```cpp
void printName(const std::string& name);
void normalizeSpeed(int& speed);
```

`printName` cannot change input string.
`normalizeSpeed` can change caller value.

## Function overloads

C++ supports multiple functions with same name and different signatures.

```cpp
int area(int side);
int area(int width, int height);
```

Use overloads when behaviors are semantically same operation.

## Return alternatives

- single value return
- struct return for grouped outputs
- optional/error wrappers for failure cases

## Practical return strategy

- return plain value when operation cannot fail
- return struct when multiple outputs are naturally grouped
- return status object (or throw, based on project policy) for failure paths

## Parameter naming quality

Good parameter names reduce debugging time.
Prefer `maxRetries` over `x`, `sourcePath` over `s` when meaning matters.

---

[← Types, Variables, and Strings](./01-types-variables-and-strings.md) · [Chapter 02](./README.md)
