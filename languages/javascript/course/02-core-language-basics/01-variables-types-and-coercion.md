<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

[Home](../../../../README.md) / [JavaScript](../../README.md) / [Chapter 02](./README.md)

---

# Variables, Types, and Coercion

> JavaScript lets values move easily between types, so professional code must be explicit about meaning.

**You will learn:**
- when to use `const`, `let`, and why to avoid `var`
- all primitive types, including `null`, `undefined`, `symbol`, and `bigint`
- truthy and falsy values
- why `===` is the normal comparison operator
- safe parsing patterns for user input and API data

**Before this page, you should know:** basic JS file execution.

---

## Variables Name Values

A variable is a name that points to a value.

```javascript
const appName = "Task Tracker";
let retryCount = 0;
```

Use `const` by default. `const` means the variable name cannot be reassigned.
The value itself may still be mutable if it is an object or array.

```javascript
const tasks = [];
tasks.push("Learn types"); // allowed

// tasks = []; // not allowed
```

Use `let` when reassignment is part of the design.

```javascript
let status = "idle";
status = "loading";
```

Avoid `var` in modern code. `var` has function scope and older hoisting behavior
that creates beginner-hostile bugs.

## Primitive Types

Primitive values are not objects.

| Type | Example | Use |
|---|---|---|
| `string` | `"hello"` | text |
| `number` | `42`, `3.14`, `NaN` | most numeric work |
| `boolean` | `true`, `false` | decisions |
| `undefined` | `undefined` | value has not been assigned or property missing |
| `null` | `null` | intentional empty value |
| `bigint` | `9007199254740993n` | integers larger than safe `number` range |
| `symbol` | `Symbol("id")` | unique property keys, advanced APIs |

```javascript
console.log(typeof "x"); // "string"
console.log(typeof 10); // "number"
console.log(typeof true); // "boolean"
console.log(typeof undefined); // "undefined"
console.log(typeof 10n); // "bigint"
console.log(typeof Symbol("id")); // "symbol"
```

Historical weirdness:

```javascript
console.log(typeof null); // "object"
```

`null` is still a primitive. The `typeof null` result is a long-standing
JavaScript quirk.

## `undefined` Versus `null`

Use `undefined` to mean "not provided or not found by JavaScript."

```javascript
const user = {};
console.log(user.name); // undefined
```

Use `null` when your program intentionally stores "nothing here."

```javascript
const selectedTask = null;
```

In UI state, `null` is often clearer because it tells the reader the absence is
intentional.

## Truthy and Falsy

JavaScript conditionals convert values to booleans.

Falsy values:

```text
false
0
-0
0n
""
null
undefined
NaN
```

Everything else is truthy, including empty arrays and empty objects.

```javascript
if ([]) {
  console.log("empty array is truthy");
}

if ({}) {
  console.log("empty object is truthy");
}
```

Do not use truthiness when `0`, `""`, or `false` are valid user inputs.

Better:

```javascript
if (name.trim() === "") {
  showError("Name is required.");
}

if (count === null) {
  showError("No count selected.");
}
```

## Equality: `===` Versus `==`

Use `===` for normal code. It compares without type coercion.

```javascript
console.log(5 === "5"); // false
console.log(5 == "5"); // true
```

`==` converts types before comparing. That can hide bugs.

```javascript
console.log(false == 0); // true
console.log("" == 0); // true
console.log(null == undefined); // true
```

Default rule:

- use `===`
- use `!==`
- convert values explicitly before comparing

## Explicit Conversion

Convert strings from inputs before numeric math.

```javascript
const rawCount = "12";
const count = Number(rawCount);

if (Number.isNaN(count)) {
  throw new Error("Count must be a number.");
}
```

Integer parsing:

```javascript
const page = Number.parseInt("10", 10);
```

Boolean conversion:

```javascript
const hasTitle = Boolean(title.trim());
```

String conversion:

```javascript
const label = String(42);
```

## Parsing Form Input

Browser form values are strings.

```html
<input id="quantity" type="number">
```

```javascript
const input = document.querySelector("#quantity");
const quantity = Number(input.value);

if (!Number.isInteger(quantity) || quantity < 1) {
  throw new Error("Quantity must be a positive whole number.");
}
```

Never assume `type="number"` gives JavaScript a number. It still gives you a
string from `.value`.

## Parsing API Data

JSON parsing gives you JavaScript values, but it does not guarantee the shape you
expected.

```javascript
const data = await response.json();

if (
  typeof data !== "object" ||
  data === null ||
  typeof data.title !== "string"
) {
  throw new Error("Invalid task response.");
}
```

Validate values at boundaries: form input, URL parameters, storage, and API
responses.

## `number` Versus `bigint`

JavaScript `number` is a floating-point type. It is fine for most UI math, but
very large integers lose precision.

```javascript
console.log(Number.MAX_SAFE_INTEGER); // 9007199254740991
```

Use `bigint` for very large integer identifiers or exact integer math.

```javascript
const huge = 9007199254740993n;
```

Do not mix `number` and `bigint` in arithmetic.

## Symbols

A symbol is a unique value often used as an object key.

```javascript
const idKey = Symbol("id");
const record = {
  [idKey]: 123,
  name: "Ada"
};
```

Most beginner browser apps do not need symbols often, but you should recognize
them in libraries and advanced APIs.

## Reference Links

- [Types and Coercion Cheat Sheet](../../reference/types-and-coercion-cheat-sheet.md)
- [Errors, Warnings, and Debugging Guide](../../reference/errors-warnings-and-debugging-guide.md)

---

## Recap

- Use `const` by default and `let` for intentional reassignment.
- `null` means intentional absence; `undefined` often means missing/not provided.
- Empty arrays and objects are truthy.
- Use `===` and explicit conversion.
- Parse and validate form, URL, storage, and API data at the boundary.

## Try It Yourself

Create a form with a text input and number input. Read both values, validate that
the title is not empty and the number is a positive integer, then log a typed
object like `{ title: "Notebook", quantity: 3 }`.

---

[**Next ->** Operators, Comparisons, and Boolean Logic](./02-operators-comparisons-and-boolean-logic.md)  
[**<- Previous** Chapter Core Language Basics](./README.md)
