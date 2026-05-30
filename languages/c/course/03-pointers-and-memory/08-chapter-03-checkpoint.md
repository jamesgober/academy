<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 03](./README.md)

---

# Chapter 03 Checkpoint: Sensor Buffer

> Build a tiny heap-backed buffer and prove you understand addresses, pointers,
> output parameters, allocation failure, cleanup, and ownership.

This checkpoint is not about writing a lot of code. It is about writing C code
where every pointer has a reason to exist.

---

## What You Are Building

Build a "sensor buffer" program.

It should:

- Allocate an integer buffer on the heap
- Fill it with sensor readings
- Update readings through pointer-based functions
- Compute the average through an output parameter
- Print readings safely
- Free memory exactly once
- Set the pointer to `NULL` after cleanup

---

## Required Project Shape

```text
sensor-buffer/
|-- main.c
`-- README.md
```

Keep it one file for now. The goal is pointer mastery, not build system
complexity.

---

## Required Functions

```c
int *make_sensor_buffer(size_t count);
bool fill_sensor_buffer(int *buffer, size_t count, int start_value);
bool add_offset(int *buffer, size_t count, int offset);
bool average_sensor_buffer(const int *buffer, size_t count, double *average_out);
void print_sensor_buffer(const int *buffer, size_t count);
void free_sensor_buffer(int **buffer_ptr);
```

Use:

```c
#include <stdbool.h>
#include <stddef.h>
```

and:

```c
#include <stdio.h>
#include <stdlib.h>
```

---

## Ownership Rules

Document these in comments:

```text
make_sensor_buffer:
  returns heap memory
  caller owns returned pointer
  caller must free it
  returns NULL on failure

fill_sensor_buffer:
  borrows buffer
  writes count elements
  does not free buffer

average_sensor_buffer:
  borrows buffer
  writes to average_out
  returns false on invalid input

free_sensor_buffer:
  frees caller-owned buffer
  sets caller pointer to NULL
```

If you cannot explain ownership, do not write the function yet.

---

## Implementation Hints

Allocation:

```c
int *make_sensor_buffer(size_t count) {
    if (count == 0) {
        return NULL;
    }

    int *buffer = malloc(count * sizeof *buffer);
    if (buffer == NULL) {
        return NULL;
    }

    return buffer;
}
```

Fill:

```c
bool fill_sensor_buffer(int *buffer, size_t count, int start_value) {
    if (buffer == NULL) {
        return false;
    }

    for (size_t i = 0; i < count; i++) {
        buffer[i] = start_value + (int)i;
    }

    return true;
}
```

Average:

```c
bool average_sensor_buffer(const int *buffer, size_t count, double *average_out) {
    if (buffer == NULL || average_out == NULL || count == 0) {
        return false;
    }

    int total = 0;
    for (size_t i = 0; i < count; i++) {
        total += buffer[i];
    }

    *average_out = (double)total / (double)count;
    return true;
}
```

Cleanup:

```c
void free_sensor_buffer(int **buffer_ptr) {
    if (buffer_ptr == NULL || *buffer_ptr == NULL) {
        return;
    }

    free(*buffer_ptr);
    *buffer_ptr = NULL;
}
```

---

## Main Flow

```c
int main(void) {
    size_t count = 5;
    int *readings = make_sensor_buffer(count);
    if (readings == NULL) {
        fprintf(stderr, "failed to allocate sensor buffer\n");
        return 1;
    }

    if (!fill_sensor_buffer(readings, count, 20)) {
        free_sensor_buffer(&readings);
        return 1;
    }

    if (!add_offset(readings, count, 3)) {
        free_sensor_buffer(&readings);
        return 1;
    }

    print_sensor_buffer(readings, count);

    double average = 0.0;
    if (average_sensor_buffer(readings, count, &average)) {
        printf("average: %.2f\n", average);
    }

    free_sensor_buffer(&readings);

    if (readings == NULL) {
        printf("buffer cleaned up\n");
    }

    return 0;
}
```

---

## Compile Commands

Basic strict build:

```bash
cc -Wall -Wextra -Wpedantic -std=c17 main.c -o sensor-buffer
```

With AddressSanitizer when available:

```bash
cc -Wall -Wextra -Wpedantic -std=c17 -fsanitize=address -g main.c -o sensor-buffer
```

Run:

```bash
./sensor-buffer
```

On Windows, your executable may be:

```powershell
.\sensor-buffer.exe
```

---

## Required Checks

Your program should show:

- allocation is checked for `NULL`
- every pointer parameter is validated when needed
- read-only functions use `const`
- array functions receive `count`
- output parameter writes are guarded
- every allocation has one cleanup path
- pointer is not used after cleanup
- sanitizer build does not report memory errors

---

## Reviewer Checklist

Ask:

```text
Can I identify who owns the heap buffer?
Can I identify who frees it?
Can any early return leak memory?
Can any function dereference NULL?
Can any function read past count?
Can any pointer be used after free?
Does each function have one clear job?
```

---

## What You Should Understand Now

You have practiced:

- addresses with `&`
- pointer declarations
- dereferencing with `*`
- pass-by-address
- pointer output parameters
- arrays as pointer-like function parameters
- `malloc`
- `free`
- null checks
- ownership comments
- sanitizer-friendly cleanup

That is the foundation of serious C.

---

## Next

Continue to [Chapter 04: Structs, Arrays, And Strings](../04-structs-arrays-and-strings/README.md).

---

[**Next ->** Chapter 04](../04-structs-arrays-and-strings/README.md)  
[**<- Previous** Avoiding Leaks, Dangling Pointers, And Double Free](./07-avoiding-leaks-dangling-pointers-and-double-free.md)
