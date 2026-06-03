<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

[Home](../../../../README.md) / [C++](../../README.md) / [Chapter 02](./README.md)

---

# Chapter 02 Checkpoint

Chapter 02 taught the everyday building blocks of C++:

- values and types
- strings and numbers
- functions
- conditionals
- loops
- vectors
- beginner input validation

This checkpoint combines them into one small real program: a score analyzer.

The program will:

- store several scores
- calculate the total
- calculate the average
- find the highest score
- convert the average into a letter grade
- print a clear report

That is the shape of many real programs: collect data, process it, make a
decision, and display a result.

## Final Program Preview

You are building toward output like this:

```text
Score report
------------
Scores: 88 72 95 100 67
Count: 5
Total: 422
Average: 84.4
Highest: 100
Grade: B
Status: passing
```

## Step 1: Start With Data

Create `scores.cpp`:

```cpp
#include <iostream>
#include <string>
#include <vector>

int main() {
    std::vector<int> scores {88, 72, 95, 100, 67};

    std::cout << "Score report\n";
    std::cout << "------------\n";

    return 0;
}
```

Compile early:

```bash
g++ -std=c++20 -Wall -Wextra -Wpedantic -g scores.cpp -o scores
```

Run:

```bash
./scores
```

PowerShell:

```powershell
.\scores.exe
```

## Step 2: Print The Scores

Add a helper function above `main`:

```cpp
void print_scores(const std::vector<int>& scores) {
    std::cout << "Scores:";

    for (int score : scores) {
        std::cout << ' ' << score;
    }

    std::cout << '\n';
}
```

Call it from `main`:

```cpp
int main() {
    std::vector<int> scores {88, 72, 95, 100, 67};

    std::cout << "Score report\n";
    std::cout << "------------\n";
    print_scores(scores);

    return 0;
}
```

Why `const std::vector<int>&`?

```text
const  = the function promises not to change the vector
&      = pass the existing vector instead of copying it
```

This is a common C++ habit: large read-only values are often passed as
`const` references.

## Step 3: Calculate A Total

Add:

```cpp
int total_score(const std::vector<int>& scores) {
    int total = 0;

    for (int score : scores) {
        total += score;
    }

    return total;
}
```

Then use it:

```cpp
int total = total_score(scores);
std::cout << "Total: " << total << '\n';
```

The loop starts with `total` at zero, then adds each score.

```text
start: 0
add 88 -> 88
add 72 -> 160
add 95 -> 255
add 100 -> 355
add 67 -> 422
```

## Step 4: Calculate An Average

An average can have a decimal part, so use `double`:

```cpp
double average_score(const std::vector<int>& scores) {
    if (scores.empty()) {
        return 0.0;
    }

    int total = total_score(scores);
    return static_cast<double>(total) / scores.size();
}
```

The guard clause handles an empty vector. Without it, dividing by zero would be
a serious bug.

`static_cast<double>(total)` tells C++ to perform floating-point division.
Without that cast, integer division can throw away the decimal part.

Use it:

```cpp
double average = average_score(scores);
std::cout << "Average: " << average << '\n';
```

## Step 5: Find The Highest Score

Add:

```cpp
int highest_score(const std::vector<int>& scores) {
    if (scores.empty()) {
        return 0;
    }

    int highest = scores[0];

    for (int score : scores) {
        if (score > highest) {
            highest = score;
        }
    }

    return highest;
}
```

This function uses the first score as the starting best value, then checks each
score against it.

```text
highest starts as 88
72 is not higher
95 is higher, so highest becomes 95
100 is higher, so highest becomes 100
67 is not higher
```

Use it:

```cpp
std::cout << "Highest: " << highest_score(scores) << '\n';
```

## Step 6: Convert Average To A Grade

Add:

```cpp
char grade_for(double average) {
    if (average >= 90.0) {
        return 'A';
    }

    if (average >= 80.0) {
        return 'B';
    }

    if (average >= 70.0) {
        return 'C';
    }

    if (average >= 60.0) {
        return 'D';
    }

    return 'F';
}
```

This is a clean chain of guard-style decisions. Each `return` exits the
function, so you do not need `else` after every branch.

Use it:

```cpp
char grade = grade_for(average);
std::cout << "Grade: " << grade << '\n';
```

## Step 7: Print A Status

Use a simple ternary for a simple two-way choice:

```cpp
std::string status = average >= 60.0 ? "passing" : "not passing";
std::cout << "Status: " << status << '\n';
```

The ternary operator is best when both outcomes are short and obvious.

```text
condition ? value_when_true : value_when_false
```

For bigger decisions, prefer `if` statements.

## Complete Version

```cpp
#include <iostream>
#include <string>
#include <vector>

void print_scores(const std::vector<int>& scores) {
    std::cout << "Scores:";

    for (int score : scores) {
        std::cout << ' ' << score;
    }

    std::cout << '\n';
}

int total_score(const std::vector<int>& scores) {
    int total = 0;

    for (int score : scores) {
        total += score;
    }

    return total;
}

double average_score(const std::vector<int>& scores) {
    if (scores.empty()) {
        return 0.0;
    }

    int total = total_score(scores);
    return static_cast<double>(total) / scores.size();
}

int highest_score(const std::vector<int>& scores) {
    if (scores.empty()) {
        return 0;
    }

    int highest = scores[0];

    for (int score : scores) {
        if (score > highest) {
            highest = score;
        }
    }

    return highest;
}

char grade_for(double average) {
    if (average >= 90.0) {
        return 'A';
    }

    if (average >= 80.0) {
        return 'B';
    }

    if (average >= 70.0) {
        return 'C';
    }

    if (average >= 60.0) {
        return 'D';
    }

    return 'F';
}

int main() {
    std::vector<int> scores {88, 72, 95, 100, 67};

    std::cout << "Score report\n";
    std::cout << "------------\n";

    print_scores(scores);

    int total = total_score(scores);
    double average = average_score(scores);
    char grade = grade_for(average);
    std::string status = average >= 60.0 ? "passing" : "not passing";

    std::cout << "Count: " << scores.size() << '\n';
    std::cout << "Total: " << total << '\n';
    std::cout << "Average: " << average << '\n';
    std::cout << "Highest: " << highest_score(scores) << '\n';
    std::cout << "Grade: " << grade << '\n';
    std::cout << "Status: " << status << '\n';

    return 0;
}
```

## Must-Be-Able Checklist

You are ready for Chapter 03 when you can explain:

- why `scores` is a `std::vector<int>`
- why totals are `int` but averages are `double`
- why large read-only parameters use `const std::vector<int>&`
- how a range loop visits every score
- how `total += score` accumulates a result
- why `scores.empty()` prevents a bug
- why `static_cast<double>` changes the division behavior
- when an `if` chain is clearer than a `switch`
- when a ternary is readable

## Stretch Practice

Improve the program one step at a time:

- Add another score and confirm the total changes.
- Change all scores to failing values and confirm the status changes.
- Add a `lowest_score` function.
- Add a `count_passing_scores` function.
- Print `No scores available` if the vector is empty.

Compile after every small change. The habit matters as much as the final code.

---

[**Next ->** Track Overview](../../README.md)  
[**<- Previous** Loops and Iteration Patterns](./04-loops-and-iteration-patterns.md)
