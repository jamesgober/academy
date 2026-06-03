<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

[Home](../../../../README.md) / [C++](../../README.md) / [Chapter 02](./README.md)

---

# Loops And Iteration Patterns

> Loops repeat work. In C++, your most common beginner loops will count, walk
> through `std::vector`, and process input until there is nothing left to do.

**You will learn:**
- `for` loops
- `while` loops
- range-based `for`
- how to loop through vectors
- when to use indexes
- why signed/unsigned warnings happen
- how to avoid infinite loops

**Before this page, you should know:** [Conditionals](./03-conditionals-if-else-if-ternary-switch.md)

---

## Counting `for` Loop

```cpp
for (int i = 0; i < 5; ++i) {
    std::cout << i << '\n';
}
```

Parts:

```text
int i = 0   start
i < 5       keep going while true
++i        update after each loop
```

Output:

```text
0
1
2
3
4
```

---

## `while` Loop

Use `while` when you do not know the number of repetitions upfront.

```cpp
int value = 0;

while (value != -1) {
    std::cout << "Enter value or -1 to stop: ";
    std::cin >> value;

    if (!std::cin) {
        std::cout << "Invalid input\n";
        return 1;
    }
}
```

Risk:

```text
If the condition never becomes false, the loop never ends.
```

---

## `std::vector`

Include:

```cpp
#include <vector>
```

Create:

```cpp
std::vector<int> scores{90, 75, 100};
```

Add:

```cpp
scores.push_back(88);
```

Size:

```cpp
std::cout << scores.size() << '\n';
```

---

## Range-Based `for`

Use this when you need each item and not the index.

```cpp
for (int score : scores) {
    std::cout << score << '\n';
}
```

For larger objects, avoid copying:

```cpp
for (const std::string& name : names) {
    std::cout << name << '\n';
}
```

Read:

```text
for each name in names
```

---

## Modify Each Item

Use non-const reference:

```cpp
for (int& score : scores) {
    if (score > 100) {
        score = 100;
    }
}
```

`int&` means the loop variable refers to the actual vector element.

Without `&`, you modify a copy.

---

## Index Loop

Use indexes when you need positions.

```cpp
for (std::size_t i = 0; i < scores.size(); ++i) {
    std::cout << i << ": " << scores[i] << '\n';
}
```

`scores.size()` returns `std::size_t`, an unsigned type.

That is why this can warn:

```cpp
for (int i = 0; i < scores.size(); ++i) {
}
```

Use `std::size_t` for indexes into standard containers.

---

## Bounds

This is unsafe if the vector is empty:

```cpp
std::cout << scores[0] << '\n';
```

Check first:

```cpp
if (!scores.empty()) {
    std::cout << scores[0] << '\n';
}
```

Use `.at()` for bounds checking:

```cpp
std::cout << scores.at(0) << '\n';
```

`.at()` throws if the index is invalid.

---

## Accumulation

Manual:

```cpp
int total = 0;

for (int score : scores) {
    total += score;
}
```

Average:

```cpp
if (!scores.empty()) {
    double average = static_cast<double>(total) / scores.size();
    std::cout << average << '\n';
}
```

The `static_cast<double>` avoids integer division.

---

## Real Example: Filter Valid Scores

```cpp
#include <iostream>
#include <vector>

std::vector<int> valid_scores(const std::vector<int>& scores) {
    std::vector<int> result;

    for (int score : scores) {
        if (score >= 0 && score <= 100) {
            result.push_back(score);
        }
    }

    return result;
}

int main() {
    std::vector<int> raw{100, -1, 80, 140, 60};
    std::vector<int> valid = valid_scores(raw);

    for (int score : valid) {
        std::cout << score << '\n';
    }
}
```

This pattern appears constantly:

```text
create result
loop over input
keep items matching a rule
return result
```

---

## Common Mistakes

### Mistake 1: Infinite Loop

```cpp
int i = 0;
while (i < 10) {
    std::cout << i << '\n';
}
```

`i` never changes.

### Mistake 2: Off-By-One

Wrong:

```cpp
for (std::size_t i = 0; i <= scores.size(); ++i) {
    std::cout << scores[i] << '\n';
}
```

Use `<`, not `<=`.

### Mistake 3: Copying Large Objects

```cpp
for (std::string name : names) {
}
```

Prefer:

```cpp
for (const std::string& name : names) {
}
```

---

## Chapter Checkpoint

You should now be able to answer:

- When is a `for` loop useful?
- When is a `while` loop useful?
- What is a range-based `for` loop?
- How do you modify vector elements in a loop?
- Why use `std::size_t` for indexes?
- What is an off-by-one error?
- Why check `.empty()` before reading the first element?

---

## Recap

- Loops repeat work.
- Use range-based `for` when you do not need indexes.
- Use `const&` to avoid copying large objects.
- Use `&` to modify elements.
- Use `std::size_t` for container indexes.
- Watch for infinite loops and off-by-one errors.

## Try It Yourself

Write a program that:

- stores scores in `std::vector<int>`
- filters invalid scores
- prints valid scores
- prints the average
- handles an empty vector safely

---

[**Next ->** Chapter 02 Checkpoint](./05-chapter-02-checkpoint.md)  
[**<- Previous** Conditionals: If, Else-If, Ternary, Switch](./03-conditionals-if-else-if-ternary-switch.md)
