<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

[Home](../../../../README.md) / [JavaScript](../../README.md) / [Chapter 02](./README.md)

---

# Arrays, Objects, and Common Mutations

> Frontend JavaScript is mostly transforming arrays and objects, then rendering the result into the DOM.

**You will learn:**
- how arrays store ordered data and objects store named data
- which array methods mutate and which return new values
- how to use `push`, `pop`, `slice`, `splice`, `map`, `filter`, `find`, `some`, `every`, and `reduce`
- how object property access, spread, destructuring, and nested updates work
- how these patterns show up in real UI state

**Before this page, you should know:** [Loops, Iteration, and Control Flow](./04-loops-iteration-and-control-flow.md)

---

## Arrays: Ordered Lists

An array is an ordered list of values. The first value is at index `0`.

```javascript
const tasks = ["learn arrays", "build UI", "ship project"];

console.log(tasks[0]); // "learn arrays"
console.log(tasks.length); // 3
```

Use arrays for lists: tasks, users, products, messages, search results, rows,
cards, menu items, and API response collections.

## Mutating Versus Non-Mutating Methods

A mutating method changes the original array. A non-mutating method returns a
new value and leaves the original array alone.

```text
Original array
    |
    |-- mutating method      -> same array changed
    `-- non-mutating method  -> new array/value returned
```

This distinction matters in frontend apps because UI state is often shared. A
hidden mutation can make the page update in surprising ways.

## Add and Remove Items

`push` adds to the end and mutates the array.

```javascript
const tasks = ["write HTML", "style page"];
tasks.push("add JavaScript");

console.log(tasks); // ["write HTML", "style page", "add JavaScript"]
```

`pop` removes from the end and returns the removed item.

```javascript
const tasks = ["write HTML", "style page", "add JavaScript"];
const lastTask = tasks.pop();

console.log(lastTask); // "add JavaScript"
console.log(tasks); // ["write HTML", "style page"]
```

`unshift` adds to the beginning. `shift` removes from the beginning.

```javascript
const queue = ["second"];
queue.unshift("first");
const next = queue.shift();

console.log(next); // "first"
console.log(queue); // ["second"]
```

Use `push`/`pop` for stack-like behavior. Use `shift`/`unshift` carefully on
large arrays because moving the first item can require re-indexing the rest.

## Copy Part of an Array with `slice`

`slice(start, end)` returns a shallow copy. The `end` index is not included.

```javascript
const names = ["Ada", "Grace", "Linus", "Brendan"];

console.log(names.slice(0, 2)); // ["Ada", "Grace"]
console.log(names.slice(2)); // ["Linus", "Brendan"]
console.log(names.slice(-1)); // ["Brendan"]
console.log(names); // original unchanged
```

Use `slice` when you want part of a list without changing the original.

## Insert or Remove with `splice`

`splice(start, deleteCount, ...items)` mutates the original array.

```javascript
const names = ["Ada", "Grace", "Brendan"];

names.splice(2, 0, "Linus"); // insert at index 2
console.log(names); // ["Ada", "Grace", "Linus", "Brendan"]

const removed = names.splice(1, 1); // remove 1 item at index 1
console.log(removed); // ["Grace"]
console.log(names); // ["Ada", "Linus", "Brendan"]
```

Use `splice` when mutation is intentional. In state-driven UI code, prefer
non-mutating alternatives unless you fully control the array.

## Transform Lists with `map`

`map(callback)` creates a new array by converting each item.

```javascript
const tasks = [
  { id: 1, title: "Write HTML", done: true },
  { id: 2, title: "Add JavaScript", done: false }
];

const titles = tasks.map((task) => task.title);

console.log(titles); // ["Write HTML", "Add JavaScript"]
```

Callback parameters:

```javascript
array.map((item, index, originalArray) => {
  return item;
});
```

Most code only needs `item`. Use `index` when position matters. Rarely use the
third parameter.

## Keep Matching Items with `filter`

`filter(callback)` returns a new array containing items where the callback
returns `true`.

```javascript
const openTasks = tasks.filter((task) => !task.done);

console.log(openTasks); // [{ id: 2, title: "Add JavaScript", done: false }]
```

Use `filter` for search results, active items, completed tasks, visible rows,
and permission checks.

## Find One Item with `find`

`find(callback)` returns the first matching item or `undefined`.

```javascript
const task = tasks.find((task) => task.id === 2);

if (task) {
  console.log(task.title);
}
```

Always handle the `undefined` case. A missing item is normal in real apps.

## Ask Yes/No Questions with `some`, `every`, and `includes`

```javascript
const hasCompleted = tasks.some((task) => task.done);
const allCompleted = tasks.every((task) => task.done);
const allowedRoles = ["admin", "editor"];
const canEdit = allowedRoles.includes("editor");
```

Use these when the result should be a boolean.

## Summarize with `reduce`

`reduce(callback, initialValue)` turns an array into one final value.

```javascript
const cart = [
  { name: "Notebook", price: 5 },
  { name: "Pen", price: 2 }
];

const total = cart.reduce((sum, item) => {
  return sum + item.price;
}, 0);

console.log(total); // 7
```

Callback parameters:

```javascript
array.reduce((accumulator, item, index, originalArray) => {
  return accumulator;
}, initialValue);
```

The accumulator is the value you are building. The initial value prevents
empty-array bugs and makes the result type obvious.

Real grouping example:

```javascript
const tasksByStatus = tasks.reduce((groups, task) => {
  const status = task.done ? "done" : "open";
  groups[status] ??= [];
  groups[status].push(task);
  return groups;
}, {});
```

Use `reduce` when the output is not simply "one item becomes one item" or "keep
some items." If `map` or `filter` says the idea more clearly, use those instead.

## Objects: Named Data

An object stores values behind property names.

```javascript
const user = {
  id: 42,
  name: "Maya",
  role: "admin"
};

console.log(user.name);
console.log(user["role"]);
```

Dot access is common when you know the property name. Bracket access is useful
when the property name is stored in a variable.

```javascript
const field = "name";
console.log(user[field]); // "Maya"
```

## Update Objects

Direct mutation:

```javascript
const user = { name: "Maya", points: 5 };
user.points += 1;
```

Immutable update:

```javascript
const user = { name: "Maya", points: 5 };
const updatedUser = { ...user, points: user.points + 1 };
```

The spread syntax `...user` copies enumerable own properties into a new object.
The later `points` property overrides the copied one.

## Nested Updates

Nested objects need nested copies.

```javascript
const state = {
  filter: {
    search: "",
    showDone: false
  },
  tasks: []
};

const nextState = {
  ...state,
  filter: {
    ...state.filter,
    search: "rust"
  }
};
```

If you only copy the outer object, the nested object is still shared.

## Destructuring

Destructuring pulls values out of arrays or objects.

```javascript
const task = { id: 1, title: "Learn DOM", done: false };
const { title, done } = task;

const colors = ["red", "green", "blue"];
const [firstColor, secondColor] = colors;
```

Use destructuring to make function parameters clearer:

```javascript
function renderTask({ title, done }) {
  return `${done ? "[x]" : "[ ]"} ${title}`;
}
```

## Real UI State Example

This is the shape you will use in browser projects:

```javascript
const state = {
  tasks: [
    { id: 1, title: "Create form", done: true },
    { id: 2, title: "Render list", done: false }
  ],
  filter: "all"
};

function toggleTask(state, id) {
  return {
    ...state,
    tasks: state.tasks.map((task) => {
      if (task.id !== id) return task;
      return { ...task, done: !task.done };
    })
  };
}
```

This function does not touch the DOM. It only creates the next state. Later
lessons render state into real page elements.

## Quick Method Guide

| Method | Mutates? | Returns | Use for |
|---|---:|---|---|
| `push(item)` | yes | new length | add to end |
| `pop()` | yes | removed item or `undefined` | remove from end |
| `shift()` | yes | removed item or `undefined` | remove from front |
| `unshift(item)` | yes | new length | add to front |
| `splice(start, count, ...items)` | yes | removed items | insert/remove in place |
| `slice(start, end)` | no | new array | copy range |
| `map(callback)` | no | new array | transform each item |
| `filter(callback)` | no | new array | keep matching items |
| `find(callback)` | no | item or `undefined` | locate one item |
| `some(callback)` | no | boolean | any match |
| `every(callback)` | no | boolean | all match |
| `reduce(callback, initial)` | no | final value | summarize/group/build |

---

## Recap

- Arrays are ordered lists; objects are named records.
- Know which methods mutate and which return new values.
- `map`, `filter`, `find`, `some`, `every`, and `reduce` cover most UI data work.
- Use object spread for state updates, especially before rendering UI.
- Keep state transformations separate from DOM rendering.

## Try It Yourself

Create a `tasks` array with at least four task objects. Then write:

- `addTask(tasks, title)`
- `toggleTask(tasks, id)`
- `getOpenTasks(tasks)`
- `countDone(tasks)`

Make each function return a new value instead of mutating the original array.

---

[**Next ->** Functions, Objects, and Modules](../03-functions-objects-and-modules/README.md)  
[**<- Previous** Loops, Iteration, and Control Flow](./04-loops-iteration-and-control-flow.md)
