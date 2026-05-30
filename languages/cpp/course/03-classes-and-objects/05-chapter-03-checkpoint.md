<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

[Home](../../../../README.md) / [C++](../../README.md) / [Chapter 03](./README.md)

---

# Chapter 03 Checkpoint: Build A Small Task Model

> This checkpoint makes classes feel practical. You will model tasks, protect
> object state, use `const` correctly, and add a small polymorphic reporting
> interface.

**You will practice:**
- class design
- private data
- constructors
- invariants
- `const` methods
- static helpers
- composition
- simple runtime polymorphism

---

## Goal

Build a tiny task tracker model.

The app should:

- create task titles safely
- create tasks with ids and completion state
- prevent empty titles
- print tasks
- report tasks through a polymorphic interface

Keep it in one file for now:

```text
task-model/
  main.cpp
```

Compile:

```bash
g++ -std=c++20 -Wall -Wextra -Wpedantic main.cpp -o task_model
./task_model
```

---

## Complete Example

```cpp
#include <iostream>
#include <memory>
#include <string>
#include <utility>
#include <vector>

class TaskTitle {
public:
    explicit TaskTitle(std::string value)
        : value_(clean(std::move(value))) {
        if (value_.empty()) {
            value_ = "Untitled task";
        }
    }

    const std::string& value() const {
        return value_;
    }

    bool empty() const {
        return value_.empty();
    }

private:
    static std::string clean(std::string input) {
        while (!input.empty() && input.front() == ' ') {
            input.erase(input.begin());
        }

        while (!input.empty() && input.back() == ' ') {
            input.pop_back();
        }

        return input;
    }

    std::string value_;
};

class Task {
public:
    Task(int id, TaskTitle title)
        : id_(id), title_(std::move(title)) {}

    int id() const {
        return id_;
    }

    const TaskTitle& title() const {
        return title_;
    }

    bool done() const {
        return done_;
    }

    void mark_done() {
        done_ = true;
    }

    void reopen() {
        done_ = false;
    }

    void print() const {
        std::cout << "#" << id_ << " ["
                  << (done_ ? 'x' : ' ')
                  << "] " << title_.value() << '\n';
    }

private:
    int id_;
    TaskTitle title_;
    bool done_ = false;
};

class TaskReporter {
public:
    virtual ~TaskReporter() = default;
    virtual void report(const Task& task) const = 0;
};

class ConsoleTaskReporter : public TaskReporter {
public:
    void report(const Task& task) const override {
        task.print();
    }
};

class VerboseTaskReporter : public TaskReporter {
public:
    void report(const Task& task) const override {
        std::cout << "Task id=" << task.id()
                  << " done=" << (task.done() ? "yes" : "no")
                  << " title=\"" << task.title().value() << "\"\n";
    }
};

void print_tasks(
    const std::vector<Task>& tasks,
    const TaskReporter& reporter
) {
    for (const Task& task : tasks) {
        reporter.report(task);
    }
}

int main() {
    std::vector<Task> tasks;

    tasks.emplace_back(1, TaskTitle(" learn classes "));
    tasks.emplace_back(2, TaskTitle(""));
    tasks.emplace_back(3, TaskTitle("practice const methods"));

    tasks[0].mark_done();

    ConsoleTaskReporter simple;
    VerboseTaskReporter verbose;

    std::cout << "Simple report:\n";
    print_tasks(tasks, simple);

    std::cout << "\nVerbose report:\n";
    print_tasks(tasks, verbose);
}
```

---

## What This Example Teaches

`TaskTitle` protects an invariant:

```text
TaskTitle is never empty after construction.
```

`Task` owns its title directly:

```text
Task has a TaskTitle.
```

That is composition.

`TaskReporter` is a polymorphic interface:

```text
ConsoleTaskReporter is a TaskReporter.
VerboseTaskReporter is a TaskReporter.
```

`print_tasks` does not care which reporter it receives:

```cpp
void print_tasks(
    const std::vector<Task>& tasks,
    const TaskReporter& reporter
)
```

That is polymorphism with a borrowed reference. No ownership transfer is needed.

---

## Required Explanation

Answer these:

- Why is `TaskTitle::value_` private?
- What invariant does `TaskTitle` protect?
- Why is `Task::print()` marked `const`?
- Why does `print_tasks` take `const std::vector<Task>&`?
- Why does `TaskReporter` have a virtual destructor?
- Why does `ConsoleTaskReporter::report` use `override`?
- Where is composition used?
- Where is inheritance used?

---

## Stretch 1: Add Due Dates

Create:

```cpp
class DueDate {
public:
    explicit DueDate(std::string value);
    const std::string& value() const;

private:
    std::string value_;
};
```

Then add it to `Task`.

Ask:

```text
Is a due date part of a task, or a kind of task?
```

It is part of a task, so use composition.

---

## Stretch 2: Add A JSON-Like Reporter

Create:

```cpp
class JsonLikeTaskReporter : public TaskReporter {
public:
    void report(const Task& task) const override;
};
```

Output:

```text
{"id":1,"done":true,"title":"learn classes"}
```

Keep it simple. You do not need a real JSON library yet.

---

## Self-Check

You are ready to move on when you can:

- write a class with private data and public methods
- create valid objects with constructors
- explain when a method should be `const`
- use a static helper for class-related behavior
- choose composition for "has-a"
- choose inheritance for "is-a"
- write a tiny base class with virtual functions
- use `override`

---

[**Next ->** Track Overview](../../README.md)  
[**<- Previous** Inheritance And Polymorphism Basics](./04-inheritance-and-polymorphism-basics.md)
