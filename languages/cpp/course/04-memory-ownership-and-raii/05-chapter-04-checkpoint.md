<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

[Home](../../../../README.md) / [C++](../../README.md) / [Chapter 04](./README.md)

---

# Chapter 04 Checkpoint: Build A Safe Inventory

> This checkpoint turns the memory chapter into muscle memory. You will build a
> small inventory program that uses values, references, vectors, and
> `std::unique_ptr` without manual `delete`.

**You will practice:**
- Choosing values before heap allocation
- Passing objects by `const&`
- Moving `unique_ptr` ownership into a class
- Storing owned dynamic objects in a `std::vector`
- Avoiding raw owning pointers
- Explaining lifetime in plain English

---

## Goal

Build a tiny inventory app.

The app should:

- create items
- add items to an inventory
- list all items
- find an item by id
- update quantity
- remove sold-out items
- use no manual `delete`

This is small enough for a beginner, but real enough to practice useful C++.

---

## Project Shape

For now, keep it in one file:

```text
inventory/
  main.cpp
```

Later chapters can split this into headers and source files.

Compile with:

```bash
g++ -std=c++20 -Wall -Wextra -Wpedantic main.cpp -o inventory
./inventory
```

Or with MSVC:

```powershell
cl /std:c++20 /W4 /EHsc main.cpp
.\main.exe
```

---

## Step 1: Create The Item Class

```cpp
#include <iostream>
#include <memory>
#include <string>
#include <utility>
#include <vector>

class Item {
public:
    Item(int id, std::string name, int quantity)
        : id_(id), name_(std::move(name)), quantity_(quantity) {}

    int id() const {
        return id_;
    }

    const std::string& name() const {
        return name_;
    }

    int quantity() const {
        return quantity_;
    }

    void add_stock(int amount) {
        if (amount > 0) {
            quantity_ += amount;
        }
    }

    bool sell_one() {
        if (quantity_ <= 0) {
            return false;
        }

        --quantity_;
        return true;
    }

    bool sold_out() const {
        return quantity_ == 0;
    }

    void print() const {
        std::cout << "#" << id_ << " " << name_
                  << " qty=" << quantity_ << '\n';
    }

private:
    int id_;
    std::string name_;
    int quantity_;
};
```

Ownership note:

```text
Item owns its id, name, and quantity directly.
No heap allocation is needed inside Item.
```

---

## Step 2: Build A Factory

```cpp
std::unique_ptr<Item> make_item(int id, std::string name, int quantity) {
    return std::make_unique<Item>(id, std::move(name), quantity);
}
```

Why use a factory here?

Because the inventory will store `unique_ptr<Item>` values. The factory gives
the caller a clear ownership package.

```text
make_item creates an Item and returns ownership to the caller.
```

---

## Step 3: Create The Inventory Class

```cpp
class Inventory {
public:
    void add(std::unique_ptr<Item> item) {
        items_.push_back(std::move(item));
    }

    void print_all() const {
        for (const auto& item : items_) {
            item->print();
        }
    }

private:
    std::vector<std::unique_ptr<Item>> items_;
};
```

Read this slowly:

```text
Inventory owns a vector.
The vector owns unique_ptr objects.
Each unique_ptr owns one Item.
```

Visual model:

```text
Inventory
  items_
    [0] unique_ptr -> Item #1
    [1] unique_ptr -> Item #2
    [2] unique_ptr -> Item #3
```

When `Inventory` dies, the vector dies. When the vector dies, its `unique_ptr`
elements die. When each `unique_ptr` dies, its `Item` dies.

That is RAII doing the cleanup.

---

## Step 4: Add Find Operations

Add these methods to `Inventory`:

```cpp
Item* find_by_id(int id) {
    for (const auto& item : items_) {
        if (item->id() == id) {
            return item.get();
        }
    }

    return nullptr;
}

const Item* find_by_id(int id) const {
    for (const auto& item : items_) {
        if (item->id() == id) {
            return item.get();
        }
    }

    return nullptr;
}
```

Why return a raw pointer?

Because this is a borrowed optional result.

```text
The inventory still owns the item.
The caller may receive no item.
nullptr means not found.
```

The caller must not delete the returned pointer.

---

## Step 5: Sell Items

Add:

```cpp
bool sell_one(int id) {
    Item* item = find_by_id(id);

    if (item == nullptr) {
        return false;
    }

    return item->sell_one();
}
```

This function does not own the item. It temporarily borrows it from the
inventory.

---

## Step 6: Remove Sold-Out Items

Add:

```cpp
void remove_sold_out() {
    auto new_end = std::remove_if(
        items_.begin(),
        items_.end(),
        [](const std::unique_ptr<Item>& item) {
            return item->sold_out();
        }
    );

    items_.erase(new_end, items_.end());
}
```

This needs:

```cpp
#include <algorithm>
```

Explanation:

```text
remove_if moves the keepers toward the front.
erase destroys the removed unique_ptr objects.
destroyed unique_ptr objects destroy their Items.
```

No manual `delete`.

---

## Step 7: Put It Together

Complete version:

```cpp
#include <algorithm>
#include <iostream>
#include <memory>
#include <string>
#include <utility>
#include <vector>

class Item {
public:
    Item(int id, std::string name, int quantity)
        : id_(id), name_(std::move(name)), quantity_(quantity) {}

    int id() const {
        return id_;
    }

    const std::string& name() const {
        return name_;
    }

    int quantity() const {
        return quantity_;
    }

    void add_stock(int amount) {
        if (amount > 0) {
            quantity_ += amount;
        }
    }

    bool sell_one() {
        if (quantity_ <= 0) {
            return false;
        }

        --quantity_;
        return true;
    }

    bool sold_out() const {
        return quantity_ == 0;
    }

    void print() const {
        std::cout << "#" << id_ << " " << name_
                  << " qty=" << quantity_ << '\n';
    }

private:
    int id_;
    std::string name_;
    int quantity_;
};

std::unique_ptr<Item> make_item(int id, std::string name, int quantity) {
    return std::make_unique<Item>(id, std::move(name), quantity);
}

class Inventory {
public:
    void add(std::unique_ptr<Item> item) {
        items_.push_back(std::move(item));
    }

    Item* find_by_id(int id) {
        for (const auto& item : items_) {
            if (item->id() == id) {
                return item.get();
            }
        }

        return nullptr;
    }

    const Item* find_by_id(int id) const {
        for (const auto& item : items_) {
            if (item->id() == id) {
                return item.get();
            }
        }

        return nullptr;
    }

    bool sell_one(int id) {
        Item* item = find_by_id(id);

        if (item == nullptr) {
            return false;
        }

        return item->sell_one();
    }

    void remove_sold_out() {
        auto new_end = std::remove_if(
            items_.begin(),
            items_.end(),
            [](const std::unique_ptr<Item>& item) {
                return item->sold_out();
            }
        );

        items_.erase(new_end, items_.end());
    }

    void print_all() const {
        for (const auto& item : items_) {
            item->print();
        }
    }

private:
    std::vector<std::unique_ptr<Item>> items_;
};

int main() {
    Inventory inventory;

    inventory.add(make_item(1, "Keyboard", 2));
    inventory.add(make_item(2, "Mouse", 1));
    inventory.add(make_item(3, "Monitor", 0));

    std::cout << "Initial inventory:\n";
    inventory.print_all();

    std::cout << "\nSelling one keyboard and one mouse...\n";
    inventory.sell_one(1);
    inventory.sell_one(2);

    std::cout << "\nBefore cleanup:\n";
    inventory.print_all();

    inventory.remove_sold_out();

    std::cout << "\nAfter cleanup:\n";
    inventory.print_all();
}
```

Expected output will look similar to:

```text
Initial inventory:
#1 Keyboard qty=2
#2 Mouse qty=1
#3 Monitor qty=0

Selling one keyboard and one mouse...

Before cleanup:
#1 Keyboard qty=1
#2 Mouse qty=0
#3 Monitor qty=0

After cleanup:
#1 Keyboard qty=1
```

---

## Required Explanation

After you get the code running, answer these in comments or a notebook:

- Why does `Item` store `int` and `std::string` directly instead of using pointers?
- Why does `Inventory::add` take `std::unique_ptr<Item>` by value?
- Why does `find_by_id` return `Item*` instead of `std::unique_ptr<Item>`?
- Who owns an item after `inventory.add(make_item(...))` runs?
- What destroys the items at the end of the program?
- Why is there no `delete` anywhere?

If you can answer those, the chapter worked.

---

## Stretch 1: Reject Duplicate IDs

Modify `add`:

```cpp
bool add(std::unique_ptr<Item> item) {
    if (item == nullptr) {
        return false;
    }

    if (find_by_id(item->id()) != nullptr) {
        return false;
    }

    items_.push_back(std::move(item));
    return true;
}
```

Now callers can tell whether the add worked.

---

## Stretch 2: Add A Report Function

Add:

```cpp
int total_quantity() const {
    int total = 0;

    for (const auto& item : items_) {
        total += item->quantity();
    }

    return total;
}
```

This practices read-only traversal through owned objects.

---

## Stretch 3: Replace `unique_ptr` With Values

Try this alternative:

```cpp
std::vector<Item> items_;
```

Then rewrite `add`, `find_by_id`, and `print_all`.

Question:

```text
Is unique_ptr actually needed for this inventory?
```

For this simple program, probably not. A vector of values is simpler.

That is a mature C++ lesson:

> The best ownership tool is often no pointer at all.

---

## Self-Check

You are ready to continue when you can:

- explain stack/local lifetime
- explain heap/dynamic lifetime
- choose between value, reference, pointer, `unique_ptr`, `shared_ptr`, and `weak_ptr`
- use `std::move` to transfer `unique_ptr` ownership
- avoid returning pointers/references to locals
- explain why RAII works on early returns
- compile with warnings
- run a sanitizer when available

---

## Recap

This chapter is the center of modern C++ safety.

You learned:

- values are the default
- RAII ties cleanup to object lifetime
- `unique_ptr` gives one clear owner
- `shared_ptr` gives shared lifetime
- `weak_ptr` prevents shared ownership cycles
- raw pointers are fine for non-owning optional access
- references are best for required non-owning access
- `std::vector` beats manual dynamic arrays
- lifetime bugs are easier to debug when ownership is explicit

---

[**Next ->** Track Overview](../../README.md)  
[**<- Previous** Avoiding Leaks And Lifetime Bugs](./04-avoiding-leaks-and-lifetime-bugs.md)
