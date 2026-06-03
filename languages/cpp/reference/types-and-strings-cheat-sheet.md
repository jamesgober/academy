# C++ Types And Strings Cheat Sheet

[Reference Index](./README.md) / [C++](../README.md)

Use this page when you need to choose a basic type, format text, or remember the
beginner-safe string patterns from [Types, Variables, and Strings](../course/02-core-language-basics/01-types-variables-and-strings.md).

## Common Type Choices

| Need | Prefer | Notes |
|---|---|---|
| ordinary whole numbers | `int` | Good default for counts, indexes after conversion, menu choices. |
| exact width integer | `std::int32_t`, `std::int64_t` | Include `<cstdint>`. Useful for file formats, protocols, and binary data. |
| sizes and container lengths | `std::size_t` | Returned by `std::string::size()` and `std::vector::size()`. Unsigned. |
| ordinary decimal math | `double` | Good default for measurements. Not exact for money. |
| single character | `char` | Stores one narrow character, not a full user-facing text model. |
| application text | `std::string` | Prefer over raw character arrays for beginner application code. |
| true or false | `bool` | Values are `true` and `false`. |

## Integer Examples

```cpp
int score = 95;
int attempts = 3;

if (score >= 90) {
    std::cout << "excellent\n";
}
```

Fixed-width integers require `<cstdint>`:

```cpp
#include <cstdint>

std::int64_t file_size = 9'000'000'000;
```

Use fixed-width types when the exact size matters. For ordinary beginner
program logic, `int` is usually easier.

## Floating-Point Examples

```cpp
double temperature = 72.5;
double average = 84.4;
```

Floating-point values can contain tiny rounding differences. Avoid using exact
equality for calculated decimal values:

```cpp
if (average == 84.4) {
    // Risky if average came from earlier floating-point math.
}
```

Prefer a range check when precision matters:

```cpp
if (average > 84.39 && average < 84.41) {
    std::cout << "close enough\n";
}
```

## `std::string`

Include `<string>`:

```cpp
#include <string>

std::string name = "Ada";
std::string greeting = "Hello, " + name;
```

Common operations:

```cpp
std::string text = "C++";

std::cout << text.size() << '\n';     // length
std::cout << text.empty() << '\n';    // true if length is zero

text += " course";                    // append
std::cout << text << '\n';
```

## Reading Strings

`std::cin >> name` reads one whitespace-separated word:

```cpp
std::string name;
std::cin >> name;
```

Use `std::getline` for a full line:

```cpp
std::string full_name;
std::getline(std::cin, full_name);
```

If you mix `operator>>` and `std::getline`, the leftover newline can surprise
you. A common beginner fix is to use line-based input consistently, then parse
when needed.

## Characters Versus Strings

Single quotes create a character:

```cpp
char grade = 'A';
```

Double quotes create a string literal:

```cpp
std::string grade_text = "A";
```

Do not mix them casually. `'A'` and `"A"` are different kinds of values.

## Initialization Patterns

Prefer clear initialization:

```cpp
int count = 0;
double total = 0.0;
std::string name = "Ada";
```

Braced initialization can catch some narrowing mistakes:

```cpp
int count {10};
double price {19.99};
```

Avoid using uninitialized variables:

```cpp
int count; // Bad beginner habit: value is indeterminate.
```

Initialize before use:

```cpp
int count = 0;
```

## Conversions

Convert intentionally.

```cpp
int total = 422;
int count = 5;

double average = static_cast<double>(total) / count;
```

Without the cast, integer division would produce `84` instead of `84.4`.

## Risk Notices

- `std::size_t` is unsigned, so be careful mixing it with negative `int` values.
- `double` is not exact decimal money storage.
- `char` is not a complete Unicode text solution.
- Raw character arrays require manual capacity and null-terminator care; prefer
  `std::string` until a lesson specifically needs lower-level storage.
- Uninitialized local variables are a serious source of confusing behavior.

## Related Lessons

- [Types, Variables, and Strings](../course/02-core-language-basics/01-types-variables-and-strings.md)
- [Functions, Parameters, and Returns](../course/02-core-language-basics/02-functions-parameters-and-returns.md)
- [Chapter 02 Checkpoint](../course/02-core-language-basics/05-chapter-02-checkpoint.md)

---

[Reference Index](./README.md) / [C++](../README.md)
