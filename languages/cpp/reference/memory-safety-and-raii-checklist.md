# Memory Safety And RAII Checklist

[Reference Index](./README.md) / [C++](../README.md)

---

## Related Lessons

- [Stack, Heap, Pointers, And References](../course/04-memory-ownership-and-raii/01-stack-heap-pointers-and-references.md)
- [RAII And Deterministic Cleanup](../course/04-memory-ownership-and-raii/02-raii-and-deterministic-cleanup.md)
- [Smart Pointers](../course/04-memory-ownership-and-raii/03-smart-pointers-unique-shared-weak.md)
- [Avoiding Leaks And Lifetime Bugs](../course/04-memory-ownership-and-raii/04-avoiding-leaks-and-lifetime-bugs.md)
- [Chapter 04 Checkpoint](../course/04-memory-ownership-and-raii/05-chapter-04-checkpoint.md)

---

## Ownership Questions

Ask these before writing pointer-heavy code:

```text
Who owns this object?
When is it destroyed?
Can anything still refer to it after destruction?
Can this be a value instead?
Can this be a vector instead?
```

---

## Default Ownership Choices

| Situation | Prefer | Notice |
|---|---|---|
| single object with normal lifetime | `T` | simplest and safest |
| list/dynamic array | `std::vector<T>` | avoids manual `new[]` / `delete[]` |
| read-only required access | `const T&` | no ownership transfer |
| mutable required access | `T&` | caller's object may change |
| optional borrowed access | `T*` / `const T*` | null must be checked |
| one heap owner | `std::unique_ptr<T>` | move-only ownership |
| shared lifetime | `std::shared_ptr<T>` | use only when many owners are real |
| shared observer/back-reference | `std::weak_ptr<T>` | prevents ownership cycles |

---

## RAII Pattern

RAII means:

```text
resource acquired by object
resource released by destructor
scope controls cleanup
```

Common RAII types:

| Type | Resource |
|---|---|
| `std::string` | dynamic text storage |
| `std::vector<T>` | dynamic array storage |
| `std::unique_ptr<T>` | exclusive heap object |
| `std::shared_ptr<T>` | shared heap object |
| `std::ifstream` / `std::ofstream` | file handle |
| `std::lock_guard<std::mutex>` | mutex lock |

---

## Smart Pointer Reference

### `std::unique_ptr<T>`

```cpp
auto task = std::make_unique<Task>(1, "load config");
```

Use when one owner controls the object lifetime.

Move ownership:

```cpp
queue.add(std::move(task));
```

Notice:

```text
After moving from a unique_ptr, treat it as empty.
```

### `std::shared_ptr<T>`

```cpp
auto document = std::make_shared<Document>("notes");
```

Use when several owners may independently keep the object alive.

Notice:

```text
shared_ptr is not a default replacement for unique_ptr. It makes lifetime less
obvious and can create cycles.
```

### `std::weak_ptr<T>`

```cpp
if (auto session = weak_session.lock()) {
    session->ping();
}
```

Use with `shared_ptr` when code needs to observe without keeping the object
alive.

---

## Bug Checklist

| Bug | What to look for | Safer pattern |
|---|---|---|
| leak | allocation without cleanup path | RAII owner |
| use-after-free | pointer used after owner ended | keep access inside owner lifetime |
| dangling reference | reference to local returned/stored | return value or extend lifetime |
| double delete | two raw owners of same allocation | `unique_ptr` or value |
| buffer overflow | index outside valid range | check bounds, use range-for |
| shared cycle | shared owners point at each other | make one direction `weak_ptr` |

---

## API Smells

Slow down when you see:

- raw pointer data member
- function returning raw pointer without ownership notes
- `new` and `delete` in different files
- `.get()` result stored long-term
- `shared_ptr` used because ownership is unclear
- destructor plus default copy behavior
- returning reference or pointer to a local variable

---

## Release Gate

Before calling C++ memory code done:

- no raw owning pointers in public APIs
- no manual `new` / `delete` in application logic
- warnings clean
- tests cover missing, empty, and boundary cases
- sanitizer run clean when available
- ownership comments exist where borrowing may be unclear

---

[Reference Index](./README.md) / [C++](../README.md)
