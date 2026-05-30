<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 05](./README.md)

---

# Chapter 05 Capstone: Sensor Log

> This capstone turns the C track into a small real program: structs, arrays,
> dynamic memory, validation, cleanup, strict builds, and sanitizer-friendly
> thinking.

**You will build:**
- a sensor reading model
- a dynamically allocated sensor log
- add/print/average functions
- explicit cleanup
- a strict build command
- a sanitizer command
- an ownership note

---

## Final Project Shape

```text
sensor-log/
  sensor_log.h
  sensor_log.c
  main.c
```

Compile:

```bash
gcc -std=c17 -Wall -Wextra -Wpedantic -g main.c sensor_log.c -o sensor_log
```

Run:

```bash
./sensor_log
```

On Windows PowerShell if using GCC:

```powershell
.\sensor_log.exe
```

---

## Step 1: Header

`sensor_log.h`:

```c
#ifndef SENSOR_LOG_H
#define SENSOR_LOG_H

#include <stddef.h>

struct SensorReading {
    int id;
    double temperature;
    int battery_percent;
};

struct SensorLog {
    struct SensorReading *items;
    size_t count;
    size_t capacity;
};

int sensor_log_init(struct SensorLog *log, size_t capacity);
void sensor_log_free(struct SensorLog *log);
int sensor_log_add(struct SensorLog *log, struct SensorReading reading);
void sensor_log_print(const struct SensorLog *log);
int sensor_log_average_temperature(const struct SensorLog *log, double *out_average);

#endif
```

Notice:

```text
The header declares the public shape and functions.
It does not allocate anything by itself.
```

---

## Step 2: Implementation

`sensor_log.c`:

```c
#include "sensor_log.h"

#include <stdio.h>
#include <stdlib.h>

int sensor_log_init(struct SensorLog *log, size_t capacity) {
    if (log == NULL || capacity == 0) {
        return 0;
    }

    log->items = calloc(capacity, sizeof(log->items[0]));
    if (log->items == NULL) {
        log->count = 0;
        log->capacity = 0;
        return 0;
    }

    log->count = 0;
    log->capacity = capacity;
    return 1;
}

void sensor_log_free(struct SensorLog *log) {
    if (log == NULL) {
        return;
    }

    free(log->items);
    log->items = NULL;
    log->count = 0;
    log->capacity = 0;
}

int sensor_log_add(struct SensorLog *log, struct SensorReading reading) {
    if (log == NULL || log->items == NULL) {
        return 0;
    }

    if (log->count >= log->capacity) {
        return 0;
    }

    if (reading.battery_percent < 0 || reading.battery_percent > 100) {
        return 0;
    }

    log->items[log->count] = reading;
    log->count++;
    return 1;
}

void sensor_log_print(const struct SensorLog *log) {
    if (log == NULL || log->items == NULL) {
        return;
    }

    for (size_t i = 0; i < log->count; i++) {
        const struct SensorReading *reading = &log->items[i];

        printf("sensor %d temp=%.2f battery=%d%%\n",
               reading->id,
               reading->temperature,
               reading->battery_percent);
    }
}

int sensor_log_average_temperature(const struct SensorLog *log, double *out_average) {
    if (log == NULL || log->items == NULL || out_average == NULL) {
        return 0;
    }

    if (log->count == 0) {
        return 0;
    }

    double total = 0.0;

    for (size_t i = 0; i < log->count; i++) {
        total += log->items[i].temperature;
    }

    *out_average = total / (double)log->count;
    return 1;
}
```

Ownership rule:

```text
sensor_log_init allocates log->items.
sensor_log_free releases log->items.
The caller must call sensor_log_free once for each successful init.
```

---

## Step 3: App

`main.c`:

```c
#include "sensor_log.h"

#include <stdio.h>

int main(void) {
    struct SensorLog log;

    if (!sensor_log_init(&log, 4)) {
        fprintf(stderr, "failed to initialize sensor log\n");
        return 1;
    }

    if (!sensor_log_add(&log, (struct SensorReading){.id = 1, .temperature = 72.5, .battery_percent = 90})) {
        fprintf(stderr, "failed to add sensor 1\n");
        sensor_log_free(&log);
        return 1;
    }

    if (!sensor_log_add(&log, (struct SensorReading){.id = 2, .temperature = 69.0, .battery_percent = 75})) {
        fprintf(stderr, "failed to add sensor 2\n");
        sensor_log_free(&log);
        return 1;
    }

    if (!sensor_log_add(&log, (struct SensorReading){.id = 3, .temperature = 80.25, .battery_percent = 50})) {
        fprintf(stderr, "failed to add sensor 3\n");
        sensor_log_free(&log);
        return 1;
    }

    sensor_log_print(&log);

    double average = 0.0;
    if (sensor_log_average_temperature(&log, &average)) {
        printf("average temperature=%.2f\n", average);
    }

    sensor_log_free(&log);
    return 0;
}
```

Yes, the error cleanup is repetitive.

That is part of learning C:

```text
If a function owns cleanup, every exit path must respect that ownership.
```

---

## Step 4: Strict Build

```bash
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g main.c sensor_log.c -o sensor_log
```

What this checks:

- C17 language mode
- strong warnings
- warnings fail the build
- debug symbols included

---

## Step 5: Sanitizer Run

If your compiler supports AddressSanitizer:

```bash
gcc -std=c17 -Wall -Wextra -Wpedantic -g -O1 \
    -fsanitize=address,undefined \
    -fno-omit-frame-pointer \
    main.c sensor_log.c -o sensor_log_san

./sensor_log_san
```

Expected:

```text
program output
no sanitizer error report
```

If sanitizer support is unavailable, still run the strict build and manually
review cleanup paths.

---

## Step 6: Ownership Note

Create `OWNERSHIP.md`:

```md
# Sensor Log Ownership

sensor_log_init allocates the items buffer with calloc.
sensor_log_free frees the items buffer and resets the fields.

The caller owns the SensorLog object itself.
The SensorLog owns the items buffer after successful initialization.

Every path after successful sensor_log_init must call sensor_log_free before
returning from main.
```

This forces you to explain memory ownership in plain English.

---

## Stretch 1: Grow The Buffer

Instead of rejecting when full, use `realloc` to grow.

Questions before coding:

- What happens if `realloc` fails?
- How do you avoid losing the old pointer?
- When do you update `capacity`?

Safe shape:

```c
struct SensorReading *new_items = realloc(log->items, new_capacity * sizeof(log->items[0]));
if (new_items == NULL) {
    return 0;
}

log->items = new_items;
log->capacity = new_capacity;
```

Never assign `realloc` directly to the only pointer until you know it succeeded.

---

## Stretch 2: Add Tests Without A Framework

Create `sensor_log_tests.c`:

```c
#include "sensor_log.h"

#include <assert.h>

int main(void) {
    struct SensorLog log;
    assert(sensor_log_init(&log, 2));

    assert(sensor_log_add(&log, (struct SensorReading){.id = 1, .temperature = 10.0, .battery_percent = 50}));
    assert(!sensor_log_add(&log, (struct SensorReading){.id = 2, .temperature = 20.0, .battery_percent = 101}));

    double average = 0.0;
    assert(sensor_log_average_temperature(&log, &average));
    assert(average == 10.0);

    sensor_log_free(&log);
    return 0;
}
```

Build:

```bash
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g sensor_log_tests.c sensor_log.c -o sensor_log_tests
./sensor_log_tests
```

---

## Completion Checklist

- header/source split works
- strict build passes
- sanitizer run is clean when available
- every successful init is freed
- invalid battery values are rejected
- full log is handled safely
- average handles empty log safely
- ownership note is written
- tests or manual checks cover success and failure paths

---

## Final C Sign-Off

You are ready to leave the beginner C track when you can:

- compile and run from the terminal
- read compiler errors and warnings
- use structs to group data
- use arrays and loops safely
- explain pointers without hand-waving
- allocate with `malloc`/`calloc`
- free exactly once
- avoid dangling pointers
- use strict build flags
- run sanitizers when available
- write an ownership note for heap memory

---

[**Next ->** C Reference](../../reference/README.md)  
[**<- Previous** Quality Workflow And Release Checklist](./05-quality-workflow-and-release-checklist.md)
