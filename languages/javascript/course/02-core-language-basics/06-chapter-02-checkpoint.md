<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

[Home](../../../../README.md) / [JavaScript](../../README.md) / [Chapter 02](./README.md)

---

# Chapter 02 Checkpoint

> Prove you can turn raw values into clean state before moving into functions, objects, modules, and DOM architecture.

## Skills to Demonstrate

- declare values with `const` and `let`
- distinguish `null` from `undefined`
- use `===` and explicit conversion
- write `if`, `else if`, ternary, and `switch` branches
- loop with `for...of`
- transform arrays with `map`, `filter`, `find`, and `reduce`
- update objects immutably with spread syntax

## Challenge: Task Data Toolkit

Create a file named `task-data.js`. Start with:

```javascript
const tasks = [
  { id: 1, title: "Learn types", done: true, priority: "high" },
  { id: 2, title: "Practice arrays", done: false, priority: "medium" },
  { id: 3, title: "Build UI", done: false, priority: "high" }
];
```

Write code that:

- validates every task has a non-empty title
- creates an array of open tasks
- finds the task with id `2`
- counts completed tasks
- groups tasks by priority
- creates a new array with task `2` marked done
- prints a summary string like `1 of 3 complete`

## Hints

- Use `every` for validation.
- Use `filter` for open tasks.
- Use `find` for one task.
- Use `reduce` for counts or grouping.
- Use `map` for changing one item while preserving the rest.

## Solution Direction

Your code should avoid mutating the original `tasks` array. If you run
`console.log(tasks)` at the end, task `2` should still be incomplete in the
original data.

---

## Recap

- Core JavaScript is data work: validate, branch, transform, and summarize.
- The same patterns power DOM rendering and app state.
- If array methods still feel fuzzy, revisit the arrays lesson before moving on.

## Try It Yourself

Add a `dueDate` field and write `getOverdueTasks(tasks, today)` without mutating
the original array.

---

[**Next ->** Functions, Objects, and Modules](../03-functions-objects-and-modules/README.md)  
[**<- Previous** Arrays, Objects, and Common Mutations](./05-arrays-objects-and-common-mutations.md)
