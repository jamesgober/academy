<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 02](./README.md)

---

# Chapter 02 Checkpoint

This checkpoint confirms you can read and write small C programs before moving
into pointers.

You will build a tiny garage queue report. It uses:

- variables
- functions
- parameters
- return values
- conditionals
- loops
- simple formatted output

## Program Goal

The program tracks how many cars are waiting for a garage and prints a status
for each arriving car.

Example output:

```text
Garage queue
------------
Capacity: 3
Arrivals: 5
Car 1: accepted
Car 2: accepted
Car 3: accepted
Car 4: rejected
Car 5: rejected
Final count: 3
Garage status: full
```

## Step 1: Start The File

Create `garage_queue.c`:

```c
#include <stdio.h>

int main(void) {
    int capacity = 3;
    int arrivals = 5;
    int cars = 0;

    printf("Garage queue\n");
    printf("------------\n");
    printf("Capacity: %d\n", capacity);
    printf("Arrivals: %d\n", arrivals);

    return 0;
}
```

Compile:

```bash
cc -std=c17 -Wall -Wextra -Wpedantic -g garage_queue.c -o garage_queue
```

Run:

```bash
./garage_queue
```

PowerShell:

```powershell
.\garage_queue.exe
```

## Step 2: Add A Function That Answers A Question

Add this above `main`:

```c
int has_space(int cars, int capacity) {
    return cars < capacity;
}
```

This function returns true-style or false-style integer data:

```text
1 means yes
0 means no
```

C does have a Boolean type through `<stdbool.h>`, but using `int` for simple
true/false return values is common in older C code. You will see both styles.

## Step 3: Add A Function That Prints Status

Add:

```c
void print_garage_status(int cars, int capacity) {
    if (cars == 0) {
        printf("Garage status: empty\n");
    } else if (cars >= capacity) {
        printf("Garage status: full\n");
    } else {
        printf("Garage status: spaces available\n");
    }
}
```

This function prints information but does not return a value, so its return type
is `void`.

## Step 4: Process Arrivals With A Loop

Inside `main`, after the header output:

```c
for (int car = 1; car <= arrivals; car++) {
    if (has_space(cars, capacity)) {
        cars++;
        printf("Car %d: accepted\n", car);
    } else {
        printf("Car %d: rejected\n", car);
    }
}
```

Read the loop:

```text
start car at 1
keep going while car <= arrivals
add 1 after each car
```

Read the decision:

```text
if there is space:
    increase cars
    print accepted
otherwise:
    print rejected
```

## Complete Version

```c
#include <stdio.h>

int has_space(int cars, int capacity) {
    return cars < capacity;
}

void print_garage_status(int cars, int capacity) {
    if (cars == 0) {
        printf("Garage status: empty\n");
    } else if (cars >= capacity) {
        printf("Garage status: full\n");
    } else {
        printf("Garage status: spaces available\n");
    }
}

int main(void) {
    int capacity = 3;
    int arrivals = 5;
    int cars = 0;

    printf("Garage queue\n");
    printf("------------\n");
    printf("Capacity: %d\n", capacity);
    printf("Arrivals: %d\n", arrivals);

    for (int car = 1; car <= arrivals; car++) {
        if (has_space(cars, capacity)) {
            cars++;
            printf("Car %d: accepted\n", car);
        } else {
            printf("Car %d: rejected\n", car);
        }
    }

    printf("Final count: %d\n", cars);
    print_garage_status(cars, capacity);

    return 0;
}
```

## Must-Be-Able Checklist

You are ready for pointers when you can explain:

- why `capacity`, `arrivals`, and `cars` are `int`
- what `has_space` receives as parameters
- what `has_space` returns
- why `print_garage_status` returns `void`
- how the `for` loop knows when to stop
- why `cars++` changes the garage count
- the difference between `=` and `==`
- why the `else` branch handles rejected cars

## Stretch Practice

Make one change at a time and compile after each:

- Change `capacity` to `5`.
- Change `arrivals` to `2`.
- Print `Garage status: over capacity` if `cars > capacity`.
- Add a `rejected` counter and print the final rejected count.
- Move the repeated header output into a `print_header` function.

## Reviewer Checklist

Before calling this done:

- Build with warnings enabled.
- Fix every warning.
- Run the program.
- Read the output and confirm it matches the code.
- Break one semicolon on purpose, compile, read the first error, then fix it.

---

[**Next ->** Chapter 03](../03-pointers-and-memory/README.md)  
[**<- Previous** Loops and Repetition](./04-loops-and-repetition.md)
