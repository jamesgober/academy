# Commands and Build Flags

## Basic compile

```bash
g++ main.cpp -std=c++20 -o app
```

## Strict compile

```bash
g++ main.cpp -std=c++20 -Wall -Wextra -Wpedantic -Werror -o app
```

## Sanitizer build

```bash
g++ main.cpp -std=c++20 -g -O1 -fsanitize=address -fno-omit-frame-pointer -o app
```

---

[← Reference Index](./README.md) · [C++](../README.md)
