<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

[Home](../../../../README.md) / [C++](../../README.md) / [Chapter 04](./README.md)

---

# Stack, Heap, Pointers, And References

> Modern C++ is not about using `new` everywhere. It is about knowing who owns an
> object, how long it lives, and how other code is allowed to refer to it.

**You will learn:**
- What stack lifetime means
- What heap lifetime means
- Why raw pointers are not automatically ownership
- How references differ from pointers
- Why modern C++ prefers values and RAII before manual allocation
- How to choose function parameter styles safely

**Before this page, you should know:** [Classes And Objects](../03-classes-and-objects/README.md)

---

## The Beginner Mental Model

C++ objects live somewhere and have a lifetime.

```text
Object
  |
  |-- automatic lifetime  -> usually stack/local object
  |
  `-- dynamic lifetime    -> heap allocation managed by an owner
```

The question is not just:

```text
Where is the object?
```

The better question is:

```text
Who owns this object, and when is it destroyed?
```

---

## Stack Objects

```cpp
#include <iostream>
#include <string>

int main() {
    std::string name = "Ada";
    std::cout << name << '\n';
}
```

`name` is a local object.

When `main` ends, `name` is destroyed automatically.

You do not call `delete`.

You do not manually release its internal memory.

That is the normal modern C++ starting point.

Rule:

> Prefer ordinary values when they work.

---

## Scope Controls Lifetime

```cpp
#include <iostream>
#include <string>

int main() {
    {
        std::string message = "inside block";
        std::cout << message << '\n';
    } // message is destroyed here

    // message is not available here
}
```

Visual model:

```text
enter block
  create message
  use message
leave block
  destroy message automatically
```

This automatic destruction is the foundation of RAII, which comes next.

---

## Heap Objects

Heap allocation creates an object whose lifetime is not tied to the current
scope unless an owner manages it.

Old-style raw allocation:

```cpp
std::string* name = new std::string("Ada");

std::cout << *name << '\n';

delete name;
name = nullptr;
```

This is legal C++, but it is not the beginner default.

Problem:

```text
If delete is skipped, memory leaks.
If delete happens twice, behavior is wrong.
If code uses name after delete, behavior is wrong.
```

Modern C++ usually uses smart pointers or containers instead of raw owning
pointers.

---

## Raw Pointers

A raw pointer stores an address.

```cpp
int speed = 120;
int* speed_ptr = &speed;
```

Read:

```text
speed is an int.
speed_ptr is a pointer to int.
speed_ptr stores the address of speed.
*speed_ptr is the int at that address.
```

Raw pointers can mean different things:

```text
I point to one object I do not own.
I point to an array I do not own.
I may be null.
I own this heap allocation and must delete it.
```

That ambiguity is why raw owning pointers are discouraged.

---

## References

A reference is an alias to an existing object.

```cpp
int speed = 120;
int& speed_ref = speed;

speed_ref = 150;

std::cout << speed << '\n'; // 150
```

Reference mental model:

```text
speed_ref is another name for speed.
```

References:

- must be initialized
- cannot be reseated to refer to a different object
- are usually not null
- are great for function parameters

---

## Pointer Versus Reference

| Feature | Pointer | Reference |
|---|---|---|
| Can be null | Yes | Normally no |
| Can be reseated | Yes | No |
| Syntax to access | `*ptr`, `ptr->field` | normal object syntax |
| Good for optional object | Yes | No |
| Good for required parameter | Often no, use reference | Yes |
| Indicates ownership | Not by itself | No |

Use a reference when the function requires an object:

```cpp
void print_name(const std::string& name);
```

Use a pointer when absence is meaningful:

```cpp
void print_name(const std::string* name);
```

Then check:

```cpp
if (name == nullptr) {
    return;
}
```

---

## Function Parameter Choices

```cpp
void read_only(const std::string& text);
```

Use for large objects you only read.

```cpp
void modify(std::string& text);
```

Use when the function modifies the caller's object.

```cpp
void optional(const std::string* text);
```

Use when the caller may pass no object.

```cpp
void take_ownership(std::unique_ptr<Widget> widget);
```

Use when ownership moves into the function.

```cpp
std::string make_message();
```

Return by value when creating a new object. Modern C++ makes this efficient in
normal cases.

---

## Do Not Return References To Locals

Bad:

```cpp
const std::string& make_name() {
    std::string name = "Ada";
    return name;
}
```

`name` is destroyed when the function returns.

The returned reference dangles.

Good:

```cpp
std::string make_name() {
    return "Ada";
}
```

Return by value.

---

## Beginner Ownership Rules

Start here:

```text
1. Prefer local values.
2. Prefer standard containers for groups.
3. Prefer references for required non-owning access.
4. Prefer pointers only when null/reseating is meaningful.
5. Prefer smart pointers for dynamic ownership.
6. Avoid raw owning new/delete in beginner code.
```

If you write `new`, pause and ask:

```text
Could this be a local object?
Could this be a std::vector?
Could this be std::unique_ptr?
```

---

## Mini Project: Parameter Style Audit

For each function, choose a parameter style:

```cpp
void print_report(??? report);
void rename_user(??? user, std::string new_name);
void maybe_print_user(??? user);
void store_widget(??? widget);
```

Suggested answers:

```cpp
void print_report(const Report& report);
void rename_user(User& user, std::string new_name);
void maybe_print_user(const User* user);
void store_widget(std::unique_ptr<Widget> widget);
```

Explain why each one communicates ownership or non-ownership.

---

## Chapter Checkpoint

You should now be able to answer:

- What does automatic lifetime mean?
- Why are stack/local objects usually preferred?
- What does a raw pointer store?
- Why is raw pointer ownership ambiguous?
- How is a reference different from a pointer?
- When should a function use `const T&`?
- When does `T*` make sense?
- Why should you not return a reference to a local variable?

---

## Recap

- C++ objects have lifetimes.
- Scope automatically destroys local objects.
- Raw pointers store addresses but do not clearly express ownership.
- References are aliases for existing objects.
- Use references for required non-owning parameters.
- Use smart pointers for dynamic ownership.
- Avoid raw owning `new` and `delete` while learning modern C++.

## Try It Yourself

Write three functions:

```cpp
void print_score(const int& score);
void add_bonus(int& score, int bonus);
void maybe_print_score(const int* score);
```

Call all three from `main` and explain which can modify the caller's value.

---

[**Next ->** RAII And Deterministic Cleanup](./02-raii-and-deterministic-cleanup.md)  
[**<- Previous** Chapter Start](./README.md)
