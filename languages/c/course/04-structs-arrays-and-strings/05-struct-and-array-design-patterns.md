<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 04](./README.md)

---

# Struct And Array Design Patterns

> Clean C data layout is a safety decision. The shape of your structs and arrays
> decides where bounds, ownership, and validation have to live.

**You will learn:**
- How to use structs with fixed-size fields
- How to use arrays of structs
- How to track count and capacity
- How to write helper functions around a data layout
- When fixed buffers are simpler than heap allocation
- What design checks prevent common C memory bugs

**Before this page, you should know:** [Safe String Handling Patterns](./04-safe-string-handling-patterns.md)

---

## Pattern 1: Struct With Fixed-Size String Field

```c
#define CAR_NAME_SIZE 32

struct Car {
    char name[CAR_NAME_SIZE];
    int speed;
};
```

Good for:

- small beginner projects
- bounded labels or names
- simple storage
- avoiding heap ownership complexity

Tradeoff:

- every `Car` reserves 32 chars even for short names
- names longer than 31 visible characters do not fit

Remember:

```text
32 slots means 31 visible characters plus '\0'
```

---

## Add A Constructor-Like Helper

C does not have constructors, but you can write initialization functions.

```c
bool car_init(struct Car *car, const char *name, int speed) {
    if (car == NULL || name == NULL) {
        return false;
    }

    if (speed < 0) {
        return false;
    }

    int written = snprintf(car->name, sizeof car->name, "%s", name);
    if (written < 0 || (size_t)written >= sizeof car->name) {
        return false;
    }

    car->speed = speed;
    return true;
}
```

`car->name` means:

```text
access field name through pointer car
```

It is shorthand for:

```c
(*car).name
```

Use `->` when you have a pointer to a struct.

---

## Pattern 2: Array Of Structs

```c
#define GARAGE_CAPACITY 10

struct Car garage[GARAGE_CAPACITY];
size_t car_count = 0;
```

Visual model:

```text
garage capacity: 10
car_count:       how many slots are currently used

Index: 0 1 2 3 4 5 6 7 8 9
Used:  yes yes no no no no no no no no
```

Capacity and count are different.

| Term | Meaning |
|---|---|
| capacity | total slots available |
| count | slots currently used |

---

## Add To Fixed-Capacity Array

```c
bool garage_add(
    struct Car garage[],
    size_t capacity,
    size_t *count,
    const char *name,
    int speed
) {
    if (garage == NULL || count == NULL) {
        return false;
    }

    if (*count >= capacity) {
        return false;
    }

    if (!car_init(&garage[*count], name, speed)) {
        return false;
    }

    *count = *count + 1;
    return true;
}
```

What this protects:

- no write past array capacity
- no invalid car inserted
- count updates only after successful initialization

---

## Print Array Of Structs

```c
void garage_print(const struct Car garage[], size_t count) {
    if (garage == NULL) {
        return;
    }

    for (size_t i = 0; i < count; i++) {
        printf("%zu: %s at %d mph\n", i, garage[i].name, garage[i].speed);
    }
}
```

Use `const` because printing does not modify the garage.

---

## Pattern 3: Struct That Owns The Array

Instead of passing array, capacity, and count separately everywhere:

```c
#define GARAGE_CAPACITY 10

struct Garage {
    struct Car cars[GARAGE_CAPACITY];
    size_t count;
};
```

Initialize:

```c
void garage_init(struct Garage *garage) {
    if (garage == NULL) {
        return;
    }

    garage->count = 0;
}
```

Add:

```c
bool garage_add_car(struct Garage *garage, const char *name, int speed) {
    if (garage == NULL) {
        return false;
    }

    if (garage->count >= GARAGE_CAPACITY) {
        return false;
    }

    if (!car_init(&garage->cars[garage->count], name, speed)) {
        return false;
    }

    garage->count++;
    return true;
}
```

This keeps the count next to the data it describes.

---

## Fixed Buffer Versus Heap

Fixed buffer:

```c
char name[32];
```

Heap pointer:

```c
char *name;
```

Beginner guidance:

| Choice | Use when |
|---|---|
| Fixed buffer | Maximum size is clear and small |
| Heap pointer | Size must vary widely or data outlives container rules |

Fixed buffers are often easier for learners because:

- no separate allocation
- no separate free
- fewer ownership questions

But fixed buffers still need bounds checks.

---

## Design Checklist

For every struct:

```text
What fields can callers write?
What values are invalid?
Where is validation done?
Are string capacities named constants?
Does every string write know destination size?
Does every array have a count?
Does every count have a capacity?
```

For every array:

```text
Where is capacity stored?
Where is current count stored?
Who increments count?
What happens when full?
Can any function write past count or capacity?
```

---

## Mini Project: Garage

Build:

```c
#define CAR_NAME_SIZE 32
#define GARAGE_CAPACITY 5

struct Car {
    char name[CAR_NAME_SIZE];
    int speed;
};

struct Garage {
    struct Car cars[GARAGE_CAPACITY];
    size_t count;
};
```

Functions:

```c
bool car_init(struct Car *car, const char *name, int speed);
void garage_init(struct Garage *garage);
bool garage_add_car(struct Garage *garage, const char *name, int speed);
void garage_print(const struct Garage *garage);
```

Test manually:

- add valid cars
- reject empty or too-long names
- reject negative speed
- reject adding past capacity
- print only `count` cars

---

## Chapter Checkpoint

You should now be able to answer:

- Why use named constants for buffer sizes?
- Why does `char name[32]` fit only 31 visible characters?
- What does `car->name` mean?
- Why should count and capacity both exist?
- Why should count update only after successful insert?
- When is a fixed buffer simpler than heap memory?
- What safety checks belong around arrays of structs?

---

## Recap

- Struct layout is a safety decision.
- Fixed-size string fields need explicit capacities.
- Arrays of structs need count and capacity.
- Helper functions centralize validation.
- `->` accesses fields through a struct pointer.
- Fixed buffers avoid heap ownership but still need bounds checks.

## Try It Yourself

Extend the garage project with:

```c
const struct Car *garage_find_fastest(const struct Garage *garage);
```

Return `NULL` when the garage is empty. Do not modify the garage.

---

[**Next ->** Chapter 04 Checkpoint](./06-chapter-04-checkpoint.md)  
[**<- Previous** Safe String Handling Patterns](./04-safe-string-handling-patterns.md)
