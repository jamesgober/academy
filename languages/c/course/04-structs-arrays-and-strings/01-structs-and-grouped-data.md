<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 04](./README.md)

---

# Structs And Grouped Data

> A `struct` lets you group related values into one shape. In C, structs are how
> you start modeling real things: players, sensors, records, buffers, config,
> coordinates, and more.

**You will learn:**
- What a struct is
- How to define struct fields
- How to initialize structs
- How to pass structs to functions
- When to pass a pointer to a struct
- How `.` and `->` differ
- How structs help make C code less scattered

**Before this page, you should know:** [Chapter 03: Pointers And Memory](../03-pointers-and-memory/README.md)

---

## The Beginner Mental Model

Without a struct:

```c
int player_id = 7;
int player_health = 100;
int player_score = 0;
```

Those values belong together, but C does not know that.

With a struct:

```c
struct Player {
    int id;
    int health;
    int score;
};
```

Now the data has a shape:

```text
Player
  id
  health
  score
```

---

## Define And Use A Struct

```c
#include <stdio.h>

struct Player {
    int id;
    int health;
    int score;
};

int main(void) {
    struct Player player;

    player.id = 7;
    player.health = 100;
    player.score = 0;

    printf("player %d health=%d score=%d\n",
           player.id,
           player.health,
           player.score);

    return 0;
}
```

Use `.` when you have the struct value itself:

```c
player.health
```

---

## Struct Initialization

Initialize in field order:

```c
struct Player player = {7, 100, 0};
```

Better for readability:

```c
struct Player player = {
    .id = 7,
    .health = 100,
    .score = 0
};
```

Named initialization is clearer and safer when fields are added later.

---

## `typedef` Option

Some C code uses `typedef` to avoid writing `struct` every time.

```c
typedef struct {
    int id;
    int health;
    int score;
} Player;
```

Now:

```c
Player player = {
    .id = 7,
    .health = 100,
    .score = 0
};
```

Both styles exist in real C.

This course often shows explicit `struct Name` because it makes the language
mechanics visible.

---

## Passing Structs To Functions

Passing by value copies the struct:

```c
void print_player(struct Player player) {
    printf("player %d health=%d score=%d\n",
           player.id,
           player.health,
           player.score);
}
```

This is fine for small structs.

But changes inside the function do not affect the caller:

```c
void damage_copy(struct Player player) {
    player.health -= 10;
}
```

The caller's player is unchanged.

---

## Passing A Pointer To Modify

Use a pointer when the function should modify the caller's struct:

```c
void damage_player(struct Player *player, int amount) {
    if (player == NULL) {
        return;
    }

    if (amount < 0) {
        return;
    }

    player->health -= amount;

    if (player->health < 0) {
        player->health = 0;
    }
}
```

Use `->` when you have a pointer to a struct:

```c
player->health
```

means:

```c
(*player).health
```

The arrow is shorter and more common.

---

## Complete Example

```c
#include <stdio.h>

struct Player {
    int id;
    int health;
    int score;
};

void print_player(struct Player player) {
    printf("player %d health=%d score=%d\n",
           player.id,
           player.health,
           player.score);
}

void add_score(struct Player *player, int points) {
    if (player == NULL || points < 0) {
        return;
    }

    player->score += points;
}

void damage_player(struct Player *player, int amount) {
    if (player == NULL || amount < 0) {
        return;
    }

    player->health -= amount;

    if (player->health < 0) {
        player->health = 0;
    }
}

int main(void) {
    struct Player player = {
        .id = 7,
        .health = 100,
        .score = 0
    };

    add_score(&player, 25);
    damage_player(&player, 30);
    print_player(player);

    return 0;
}
```

Compile:

```bash
gcc -Wall -Wextra -Wpedantic player.c -o player
```

---

## Structs And Arrays

You can store structs in arrays:

```c
struct Player players[3] = {
    {.id = 1, .health = 100, .score = 0},
    {.id = 2, .health = 80, .score = 10},
    {.id = 3, .health = 0, .score = 50}
};
```

Loop:

```c
for (int i = 0; i < 3; i++) {
    print_player(players[i]);
}
```

Modify one:

```c
damage_player(&players[1], 20);
```

The `&` passes the address of that array element.

---

## Common Mistakes

### Mistake 1: Using `->` On A Value

Wrong:

```c
struct Player player;
player->health = 100;
```

Use:

```c
player.health = 100;
```

### Mistake 2: Using `.` On A Pointer

Wrong:

```c
struct Player *player_ptr = &player;
player_ptr.health = 100;
```

Use:

```c
player_ptr->health = 100;
```

### Mistake 3: Forgetting To Validate Pointers

If a function accepts a pointer that may be `NULL`, check it before using `->`.

---

## Chapter Checkpoint

You should now be able to answer:

- What problem does a struct solve?
- What does `.` do?
- What does `->` do?
- When does passing a struct copy it?
- When should a function take `struct Player *player`?
- Why is named initialization clearer?
- How do you store structs in an array?

---

## Recap

- Structs group related data.
- Use `.` with struct values.
- Use `->` with pointers to structs.
- Passing a struct by value copies it.
- Passing a pointer lets a function modify the caller's struct.
- Named initializers make code easier to read.

## Try It Yourself

Create a `struct SensorReading` with:

- `int id`
- `double temperature`
- `int battery_percent`

Write:

- `print_reading`
- `update_temperature`
- an array of three readings
- a loop that prints them all

---

[**Next ->** Arrays And Indexed Access](./02-arrays-and-indexed-access.md)  
[**<- Previous** Chapter 03](../03-pointers-and-memory/README.md)
