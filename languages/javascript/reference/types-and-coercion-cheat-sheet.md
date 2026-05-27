# Types and Coercion Cheat Sheet

## Primitive types

- `string`
- `number`
- `boolean`
- `null`
- `undefined`
- `symbol`
- `bigint`

## Type checks

```javascript
typeof value
Array.isArray(value)
value === null
```

## Coercion examples

```javascript
Number("42")   // 42
String(42)      // "42"
Boolean(0)      // false
Boolean("0")    // true
```

## Safe comparison rule

Prefer `===` and `!==` unless coercion is explicitly intended.

---

[← JavaScript Reference](./README.md)
