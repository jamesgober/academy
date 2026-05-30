<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

[Home](../../../../README.md) / [C++](../../README.md) / [Chapter 04](./README.md)

---

# RAII And Deterministic Cleanup

> RAII is the heart of modern C++ resource safety: acquire a resource in an
> object, release it in that object's destructor, and let scope do the cleanup.

**You will learn:**
- What RAII means
- Why destructors matter
- How scope gives deterministic cleanup
- Why RAII is safer than scattered manual cleanup
- How standard library types already use RAII
- How RAII helps when errors or exceptions happen

**Before this page, you should know:** [Stack, Heap, Pointers, And References](./01-stack-heap-pointers-and-references.md)

---

## What RAII Means

RAII stands for:

```text
Resource Acquisition Is Initialization
```

That phrase is awkward. The idea is not.

Plain language:

```text
Put a resource inside an object.
Acquire the resource when the object is created.
Release the resource when the object is destroyed.
```

Resource examples:

- heap memory
- file handle
- mutex lock
- socket
- database connection
- temporary directory

---

## Deterministic Cleanup

```cpp
#include <fstream>
#include <string>

void write_log(const std::string& message) {
    std::ofstream file("log.txt");
    file << message << '\n';
} // file closes here
```

You do not manually close the file.

`std::ofstream` closes it in its destructor.

Visual model:

```text
enter function
  create file object
  file opens resource
  write message
leave function
  file destructor runs
  file closes resource
```

That is RAII.

---

## Standard Library RAII Examples

| Type | Resource managed |
|---|---|
| `std::string` | dynamic character storage |
| `std::vector<T>` | dynamic array storage |
| `std::unique_ptr<T>` | exclusive heap object ownership |
| `std::shared_ptr<T>` | shared heap object ownership |
| `std::fstream` | file handle |
| `std::lock_guard<std::mutex>` | mutex lock |

Modern C++ becomes much safer when you let these types own resources.

---

## Manual Cleanup Is Fragile

Risky:

```cpp
void process() {
    int* values = new int[100];

    if (something_failed()) {
        return; // leak
    }

    delete[] values;
}
```

The early return skips cleanup.

Better:

```cpp
#include <vector>

void process() {
    std::vector<int> values(100);

    if (something_failed()) {
        return; // vector still cleans itself
    }
}
```

The vector destructor runs when the function exits.

---

## Destructors

A destructor is a special member function that runs when an object is destroyed.

```cpp
#include <iostream>

class Tracer {
public:
    explicit Tracer(std::string name) : name_(std::move(name)) {
        std::cout << "construct " << name_ << '\n';
    }

    ~Tracer() {
        std::cout << "destroy " << name_ << '\n';
    }

private:
    std::string name_;
};
```

Use:

```cpp
int main() {
    Tracer first("first");

    {
        Tracer second("second");
    } // second destroyed here

} // first destroyed here
```

Destruction follows scope.

---

## RAII And Exceptions

Even if an exception leaves a scope, destructors for already-created local
objects run.

```cpp
void save_report() {
    std::ofstream file("report.txt");

    if (!file) {
        throw std::runtime_error("could not open report.txt");
    }

    file << "report\n";

    throw std::runtime_error("something failed later");
} // file still closes
```

This is one reason RAII is so important in C++.

You do not need to master exceptions yet. The key idea:

> Cleanup belongs to object lifetime, not to remembering every exit path.

---

## Lock Guard Example

Manual locking is fragile:

```cpp
mutex.lock();
do_work();
mutex.unlock();
```

If `do_work` throws or returns early, unlock may be skipped.

RAII locking:

```cpp
#include <mutex>

void safe_increment(std::mutex& mutex, int& value) {
    std::lock_guard<std::mutex> lock(mutex);
    value++;
} // lock_guard unlocks mutex here
```

`std::lock_guard` owns the lock for the current scope.

---

## RAII Design Rule

If a class owns a resource, its destructor should release it.

But beginner rule:

> Prefer standard library RAII types before writing your own resource-owning
> class.

Use:

- `std::vector` for dynamic arrays
- `std::string` for text
- `std::unique_ptr` for exclusive heap ownership
- `std::ofstream` / `std::ifstream` for files
- `std::lock_guard` for mutex locks

---

## Mini Project: Replace Manual Cleanup

Manual version:

```cpp
int* make_scores(int count) {
    return new int[count];
}

void use_scores() {
    int* scores = make_scores(10);
    scores[0] = 100;
    delete[] scores;
}
```

Modern beginner version:

```cpp
#include <vector>

std::vector<int> make_scores(int count) {
    return std::vector<int>(count);
}

void use_scores() {
    std::vector<int> scores = make_scores(10);
    scores[0] = 100;
}
```

No manual `delete[]`.

No leak on early return.

---

## Chapter Checkpoint

You should now be able to answer:

- What does RAII mean in plain language?
- What is a destructor?
- Why does scope matter?
- Why is manual cleanup fragile?
- Which standard types manage resources with RAII?
- How does RAII help with early returns?
- Why should beginners prefer `std::vector` over `new[]`?

---

## Recap

- RAII ties resource cleanup to object lifetime.
- Destructors run when objects are destroyed.
- Scope gives deterministic cleanup.
- Standard library types already use RAII.
- RAII makes early returns and exceptions safer.
- Modern C++ should prefer RAII over manual resource management.

## Try It Yourself

Write a function that creates a `std::vector<int>`, fills it, returns early under
one condition, and explain why no manual cleanup is needed.

---

[**Next ->** Smart Pointers: `unique_ptr`, `shared_ptr`, `weak_ptr`](./03-smart-pointers-unique-shared-weak.md)  
[**<- Previous** Stack, Heap, Pointers, And References](./01-stack-heap-pointers-and-references.md)
