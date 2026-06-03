<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 04](./README.md)

---

# Chapter 04 Checkpoint

This checkpoint confirms you can model structured data and handle C strings
safely before moving into build, test, and debug workflows.

You will build a small garage records model with:

- a `struct`
- fixed-size character arrays
- an array of records
- bounded string copying
- clear capacity checks
- one update function

## Program Goal

Expected output:

```text
Garage records
--------------
Ada: Roadster, 2024
Grace: Van, 2020
Updated:
Ada: Electric Roadster, 2024
Grace: Van, 2020
```

## Step 1: Define The Struct

```c
#include <stdio.h>
#include <string.h>

#define OWNER_CAPACITY 32
#define MODEL_CAPACITY 48

struct CarRecord {
    char owner[OWNER_CAPACITY];
    char model[MODEL_CAPACITY];
    int year;
};
```

The arrays are fixed-size buffers.

```text
owner can store up to 31 visible characters plus '\0'
model can store up to 47 visible characters plus '\0'
```

C strings need space for the null terminator.

## Step 2: Write A Safe Copy Helper

```c
int copy_text(char *destination, size_t destination_size, const char *source) {
    int written = snprintf(destination, destination_size, "%s", source);

    if (written < 0) {
        return 0;
    }

    return (size_t)written < destination_size;
}
```

This helper returns:

```text
1 if the full text fit
0 if formatting failed or the text was truncated
```

`snprintf` is useful because it knows the destination size.

## Step 3: Initialize A Record

```c
int init_car_record(
    struct CarRecord *record,
    const char *owner,
    const char *model,
    int year
) {
    if (record == NULL || owner == NULL || model == NULL) {
        return 0;
    }

    if (!copy_text(record->owner, sizeof record->owner, owner)) {
        return 0;
    }

    if (!copy_text(record->model, sizeof record->model, model)) {
        return 0;
    }

    record->year = year;
    return 1;
}
```

Important pointer syntax:

```text
record->owner
```

means:

```text
the owner field inside the struct pointed to by record
```

## Step 4: Update One Field

```c
int update_model(struct CarRecord *record, const char *model) {
    if (record == NULL || model == NULL) {
        return 0;
    }

    return copy_text(record->model, sizeof record->model, model);
}
```

This function changes an existing record, so it receives a pointer.

## Step 5: Print Records

```c
void print_record(const struct CarRecord *record) {
    if (record == NULL) {
        return;
    }

    printf("%s: %s, %d\n", record->owner, record->model, record->year);
}
```

The pointer is `const` because printing should not change the record.

## Complete Version

```c
#include <stdio.h>

#define OWNER_CAPACITY 32
#define MODEL_CAPACITY 48

struct CarRecord {
    char owner[OWNER_CAPACITY];
    char model[MODEL_CAPACITY];
    int year;
};

int copy_text(char *destination, size_t destination_size, const char *source) {
    int written = snprintf(destination, destination_size, "%s", source);

    if (written < 0) {
        return 0;
    }

    return (size_t)written < destination_size;
}

int init_car_record(
    struct CarRecord *record,
    const char *owner,
    const char *model,
    int year
) {
    if (record == NULL || owner == NULL || model == NULL) {
        return 0;
    }

    if (!copy_text(record->owner, sizeof record->owner, owner)) {
        return 0;
    }

    if (!copy_text(record->model, sizeof record->model, model)) {
        return 0;
    }

    record->year = year;
    return 1;
}

int update_model(struct CarRecord *record, const char *model) {
    if (record == NULL || model == NULL) {
        return 0;
    }

    return copy_text(record->model, sizeof record->model, model);
}

void print_record(const struct CarRecord *record) {
    if (record == NULL) {
        return;
    }

    printf("%s: %s, %d\n", record->owner, record->model, record->year);
}

int main(void) {
    struct CarRecord records[2];

    if (!init_car_record(&records[0], "Ada", "Roadster", 2024)) {
        printf("failed to initialize first record\n");
        return 1;
    }

    if (!init_car_record(&records[1], "Grace", "Van", 2020)) {
        printf("failed to initialize second record\n");
        return 1;
    }

    printf("Garage records\n");
    printf("--------------\n");

    for (size_t i = 0; i < 2; i++) {
        print_record(&records[i]);
    }

    if (!update_model(&records[0], "Electric Roadster")) {
        printf("failed to update model\n");
        return 1;
    }

    printf("Updated:\n");

    for (size_t i = 0; i < 2; i++) {
        print_record(&records[i]);
    }

    return 0;
}
```

## Must-Be-Able Checklist

You are ready for Chapter 05 when you can explain:

- why `owner` and `model` are arrays
- why each string buffer needs room for `'\0'`
- why `snprintf` receives the destination size
- why `init_car_record` returns success or failure
- why update functions often receive pointers
- why print functions can receive `const` pointers
- why the array loop stops at `i < 2`
- what risk appears if a source string is too long

## Stretch Practice

Make one change at a time:

- Add a `mileage` field.
- Increase the number of records to `3`.
- Add a `print_all_records` function.
- Try a model name longer than `MODEL_CAPACITY` and confirm the program reports
  failure.
- Replace the hard-coded `2` with a named constant.

---

[**Next ->** Chapter 05](../05-build-test-and-debug-basics/README.md)  
[**<- Previous** Struct and Array Design Patterns](./05-struct-and-array-design-patterns.md)
