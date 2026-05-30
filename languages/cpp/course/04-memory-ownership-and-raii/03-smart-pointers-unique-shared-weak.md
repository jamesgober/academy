<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

[Home](../../../../README.md) / [C++](../../README.md) / [Chapter 04](./README.md)

---

# Smart Pointers: `unique_ptr`, `shared_ptr`, `weak_ptr`

> A smart pointer is an object that acts like a pointer while managing ownership
> for you. It is one of the main reasons modern C++ can be much safer than old
> manual `new` / `delete` C++.

**You will learn:**
- Why smart pointers exist
- When to use `std::unique_ptr`
- How move-only ownership works
- When `std::shared_ptr` is appropriate
- Why `std::weak_ptr` exists
- How to avoid ownership cycles
- How to design function parameters around ownership

**Before this page, you should know:** [RAII And Deterministic Cleanup](./02-raii-and-deterministic-cleanup.md)

---

## Why Smart Pointers Exist

Raw heap ownership looks like this:

```cpp
Widget* widget = new Widget();

use(widget);

delete widget;
```

That code has a hidden promise:

```text
I promise to delete this exactly once on every possible path.
```

That is a lot to ask from a beginner, and honestly a lot to ask from tired
professionals too.

Smart pointers move that promise into an object.

```cpp
#include <memory>

auto widget = std::make_unique<Widget>();
use(widget.get());
```

When `widget` leaves scope, the `Widget` object is deleted automatically.

Visual model:

```text
unique_ptr object
  owns
    heap Widget

scope ends
  unique_ptr destructor runs
  heap Widget is destroyed
```

---

## The Three Main Smart Pointers

| Type | Ownership meaning | Beginner default? |
|---|---|---|
| `std::unique_ptr<T>` | Exactly one owner | Yes |
| `std::shared_ptr<T>` | Multiple owners share lifetime | Sometimes |
| `std::weak_ptr<T>` | Watches a shared object without owning it | Only with `shared_ptr` designs |

Start with this rule:

> Use `std::unique_ptr` unless you can explain why multiple owners truly need
> shared lifetime.

---

## `std::unique_ptr`: One Owner

`std::unique_ptr<T>` means:

```text
This object has one clear owner.
When the owner dies, the object dies.
```

Example:

```cpp
#include <iostream>
#include <memory>
#include <string>
#include <utility>

class Profile {
public:
    explicit Profile(std::string name) : name_(std::move(name)) {}

    void print() const {
        std::cout << "Profile: " << name_ << '\n';
    }

private:
    std::string name_;
};

int main() {
    auto profile = std::make_unique<Profile>("Ada");
    profile->print();
}
```

Use `->` because the smart pointer points to an object:

```cpp
profile->print();
```

That is similar to:

```cpp
(*profile).print();
```

---

## Prefer `make_unique`

Prefer:

```cpp
auto profile = std::make_unique<Profile>("Ada");
```

Avoid:

```cpp
std::unique_ptr<Profile> profile(new Profile("Ada"));
```

`make_unique` is shorter, clearer, and safer when more complicated expressions
are involved.

---

## `unique_ptr` Cannot Be Copied

This will not compile:

```cpp
auto first = std::make_unique<Profile>("Ada");
auto second = first; // error
```

Why?

Because if copying were allowed, two owners would both think they should delete
the same object.

Bad mental model:

```text
first  ---- owns ---- Profile
second ---- owns ---- same Profile

Who deletes it?
Both? That would be a disaster.
```

So C++ says no.

---

## Move Ownership

You can move a `unique_ptr`.

```cpp
auto first = std::make_unique<Profile>("Ada");
auto second = std::move(first);
```

After the move:

```text
second owns the Profile.
first is empty.
```

Complete example:

```cpp
#include <iostream>
#include <memory>
#include <string>
#include <utility>

class Profile {
public:
    explicit Profile(std::string name) : name_(std::move(name)) {}

    void print() const {
        std::cout << name_ << '\n';
    }

private:
    std::string name_;
};

int main() {
    auto first = std::make_unique<Profile>("Ada");
    auto second = std::move(first);

    if (first == nullptr) {
        std::cout << "first no longer owns anything\n";
    }

    second->print();
}
```

`std::move` does not move bytes by itself. It says:

```text
It is okay to steal resources from this object.
```

For `unique_ptr`, that means ownership transfers.

---

## Returning `unique_ptr`

Factories often return `unique_ptr`.

```cpp
#include <memory>
#include <string>
#include <utility>

class Profile {
public:
    explicit Profile(std::string name) : name_(std::move(name)) {}

private:
    std::string name_;
};

std::unique_ptr<Profile> make_profile(std::string name) {
    return std::make_unique<Profile>(std::move(name));
}

int main() {
    auto profile = make_profile("Grace");
}
```

The function creates an object and gives ownership to the caller.

---

## Passing `unique_ptr` To Functions

There are three common choices.

### Borrow Without Ownership

```cpp
void print_profile(const Profile& profile) {
    profile.print();
}

auto profile = std::make_unique<Profile>("Ada");
print_profile(*profile);
```

Use this when the function only needs to use the object temporarily.

### Optional Borrow

```cpp
void maybe_print_profile(const Profile* profile) {
    if (profile == nullptr) {
        return;
    }

    profile->print();
}

auto profile = std::make_unique<Profile>("Ada");
maybe_print_profile(profile.get());
```

Use `.get()` to pass a non-owning raw pointer.

Important:

```text
.get() does not give away ownership.
```

### Take Ownership

```cpp
void store_profile(std::unique_ptr<Profile> profile) {
    profile->print();
    // profile is destroyed at the end unless moved somewhere else
}

auto profile = std::make_unique<Profile>("Ada");
store_profile(std::move(profile));
```

Passing by value means the function receives ownership.

After `std::move(profile)`, the caller should treat `profile` as empty.

---

## `std::shared_ptr`: Shared Lifetime

`std::shared_ptr<T>` means:

```text
Multiple owners may keep this object alive.
The object dies when the last shared_ptr owner is gone.
```

Example:

```cpp
#include <iostream>
#include <memory>
#include <string>
#include <utility>
#include <vector>

class Document {
public:
    explicit Document(std::string title) : title_(std::move(title)) {}

    void print() const {
        std::cout << title_ << '\n';
    }

private:
    std::string title_;
};

int main() {
    auto document = std::make_shared<Document>("Design Notes");

    std::vector<std::shared_ptr<Document>> open_tabs;
    open_tabs.push_back(document);
    open_tabs.push_back(document);

    document->print();

    std::cout << "owners: " << document.use_count() << '\n';
}
```

Visual model:

```text
document shared_ptr ----+
open_tabs[0] ---------- +---- Document
open_tabs[1] ----------+

Document stays alive while at least one shared_ptr owns it.
```

Do not build your design around constantly checking `use_count()`. It is useful
for learning and debugging, but it is rarely the best application logic.

---

## Prefer `make_shared`

Prefer:

```cpp
auto document = std::make_shared<Document>("Design Notes");
```

This is clearer and can be more efficient than separately allocating the object
and the shared ownership bookkeeping.

---

## When `shared_ptr` Is The Right Tool

Use shared ownership when the truth of the program is:

```text
There is no single obvious owner.
Several parts of the program may independently keep this object alive.
```

Examples:

- a loaded asset used by multiple game objects
- a document model used by multiple views
- a cached configuration shared by several services

Do not use `shared_ptr` just because it feels convenient.

If one object clearly owns another, use `unique_ptr` or a direct member.

---

## `std::weak_ptr`: Watching Without Owning

`std::weak_ptr<T>` works with `shared_ptr`.

It means:

```text
I know about this shared object, but I do not keep it alive.
```

To use a `weak_ptr`, you ask for a temporary `shared_ptr` with `.lock()`.

```cpp
#include <iostream>
#include <memory>

class Session {
public:
    void ping() const {
        std::cout << "session alive\n";
    }
};

void maybe_ping(std::weak_ptr<Session> weak_session) {
    if (auto session = weak_session.lock()) {
        session->ping();
    } else {
        std::cout << "session already gone\n";
    }
}

int main() {
    std::weak_ptr<Session> observer;

    {
        auto session = std::make_shared<Session>();
        observer = session;
        maybe_ping(observer);
    }

    maybe_ping(observer);
}
```

Inside the block, the session exists.

After the block, the owning `shared_ptr` is gone, so the `weak_ptr` cannot lock
it anymore.

---

## Why `weak_ptr` Matters: Cycles

Two `shared_ptr` objects can accidentally keep each other alive forever.

Bad:

```cpp
#include <memory>
#include <string>

struct Employee;

struct Department {
    std::string name;
    std::shared_ptr<Employee> lead;
};

struct Employee {
    std::string name;
    std::shared_ptr<Department> department;
};
```

Visual model:

```text
Department owns Employee
Employee owns Department

Each keeps the other alive.
The reference counts never reach zero.
```

Better:

```cpp
struct Department {
    std::string name;
    std::shared_ptr<Employee> lead;
};

struct Employee {
    std::string name;
    std::weak_ptr<Department> department;
};
```

Now the employee can refer back to the department without keeping it alive.

---

## Decision Table

| Situation | Use |
|---|---|
| Object can be a normal local/member value | `T` |
| Dynamic array/list | `std::vector<T>` |
| One owner for a heap object | `std::unique_ptr<T>` |
| Many true owners share lifetime | `std::shared_ptr<T>` |
| Non-owning back-reference into shared ownership | `std::weak_ptr<T>` |
| Required temporary access | `T&` or `const T&` |
| Optional temporary access | `T*` or `const T*` |

---

## Real Example: Task Queue Ownership

Imagine a program that builds tasks, stores them, and runs them later.

```cpp
#include <iostream>
#include <memory>
#include <string>
#include <utility>
#include <vector>

class Task {
public:
    Task(int id, std::string title)
        : id_(id), title_(std::move(title)) {}

    void run() const {
        std::cout << "#" << id_ << ": " << title_ << '\n';
    }

private:
    int id_;
    std::string title_;
};

class TaskQueue {
public:
    void add(std::unique_ptr<Task> task) {
        tasks_.push_back(std::move(task));
    }

    void run_all() const {
        for (const auto& task : tasks_) {
            task->run();
        }
    }

private:
    std::vector<std::unique_ptr<Task>> tasks_;
};

std::unique_ptr<Task> make_task(int id, std::string title) {
    return std::make_unique<Task>(id, std::move(title));
}

int main() {
    TaskQueue queue;

    queue.add(make_task(1, "read config"));
    queue.add(make_task(2, "load assets"));
    queue.add(make_task(3, "start server"));

    queue.run_all();
}
```

Notice the ownership story:

```text
make_task creates a Task.
TaskQueue::add takes ownership.
TaskQueue stores tasks as unique_ptr values.
TaskQueue destroys tasks automatically when the queue dies.
```

No manual `delete`.

No unclear owner.

---

## Common Mistakes

### Mistake 1: Calling `delete` On A Smart Pointer's Object

Wrong:

```cpp
auto task = std::make_unique<Task>(1, "bad idea");
delete task.get(); // do not do this
```

The `unique_ptr` still thinks it owns the object. When it later tries to clean
up, your program has a serious bug.

### Mistake 2: Using `shared_ptr` Everywhere

`shared_ptr` is not "safer unique_ptr."

It makes lifetime more flexible, but also less obvious.

Use it when shared lifetime is real.

### Mistake 3: Storing Raw Pointers From Temporary Owners

Wrong:

```cpp
Task* saved = nullptr;

{
    auto task = std::make_unique<Task>(1, "temporary");
    saved = task.get();
}

saved->run(); // dangling pointer
```

The `Task` was destroyed when `task` left scope.

---

## Chapter Checkpoint

You should now be able to answer:

- Why do smart pointers exist?
- Why is `unique_ptr` the beginner default for heap ownership?
- Why can a `unique_ptr` be moved but not copied?
- What does `.get()` do?
- When does passing `std::unique_ptr<T>` by value make sense?
- What does `shared_ptr` mean?
- Why can `shared_ptr` cycles leak memory?
- What problem does `weak_ptr` solve?

---

## Recap

- `unique_ptr` means one owner.
- `shared_ptr` means shared lifetime.
- `weak_ptr` observes shared ownership without extending lifetime.
- Prefer `make_unique` and `make_shared`.
- Prefer `unique_ptr` unless shared lifetime is genuinely needed.
- Use references or raw pointers for non-owning access.
- Never manually delete something owned by a smart pointer.

## Try It Yourself

Write a `Library` class that stores books as
`std::vector<std::unique_ptr<Book>>`.

Requirements:

- `Book` has a title and author.
- `Library::add` takes `std::unique_ptr<Book>`.
- `Library::print_all` prints every book.
- `make_book` returns `std::unique_ptr<Book>`.
- No `new` appears outside `make_unique`.
- No `delete` appears anywhere.

---

[**Next ->** Avoiding Leaks And Lifetime Bugs](./04-avoiding-leaks-and-lifetime-bugs.md)  
[**<- Previous** RAII And Deterministic Cleanup](./02-raii-and-deterministic-cleanup.md)
