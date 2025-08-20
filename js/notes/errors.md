# JavaScript Errors: ReferenceError, TypeError, SyntaxError

## ReferenceError

Occurs when you reference a variable that doesn't exist in the accessible scope, or when accessing a `let`/`const` variable in the Temporal Dead Zone (before initialization).

### 1) Unknown variable (not declared)

```js
console.log(x); // ReferenceError: x is not defined
```

### 2) Accessing let/const in TDZ

```js
console.log(a); // undefined (var is hoisted and initialized)
console.log(b); // ReferenceError (TDZ)
var a = 1;
let b = 2;
```

### 3) Block scope with let/const

```js
if (true) {
  let y = 10;
}
console.log(y); // ReferenceError: y is not defined
```

### 4) Function scope shadowing with TDZ

```js
let value = 1;
function demo() {
  // TDZ starts here for inner `value`
  // console.log(value); // ReferenceError (inner `let value` in TDZ)
  let value = 2;
  console.log(value); // 2
}
demo();
```

---

## TypeError

Occurs when an operation is performed on a value of an unexpected type, or when accessing a property/method on `null`/`undefined`.

### 1) Calling something that's not a function

```js
const notFn = 123;
notFn(); // TypeError: notFn is not a function
```

### 2) Accessing property on undefined/null

```js
let obj;
// console.log(obj.prop); // TypeError: Cannot read properties of undefined

let n = null;
// console.log(n.toString()); // TypeError: Cannot read properties of null
```

### 3) Invalid operations on types

```js
const num = 5;
num(); // TypeError: num is not a function

const arr = [1, 2, 3];
arr.foo(); // TypeError: arr.foo is not a function
```

---

## SyntaxError

Thrown when code cannot be parsed. Common cases:

### 1) Redeclaring `let` in the same scope

```js
let a = 1;
let a = 2; // SyntaxError: Identifier 'a' has already been declared
```

### 2) Reassigning `const`

```js
const x = 10;
x = 20; // TypeError at runtime (assignment to constant variable)
```

### 3) Declaring `const` without initialization

```js
const z; // SyntaxError: Missing initializer in const declaration
```

### 4) Unclosed braces / invalid tokens

```js
if (true) {
  console.log('hi');
// SyntaxError: Unexpected end of input (missing closing brace)
```

### 5) Invalid parameter names / duplicates in strict mode

```js
"use strict";
function f(a, a) {} // SyntaxError in strict mode: Duplicate parameter name not allowed
```

Notes:

- Some `const` violations appear as TypeError at runtime (e.g., reassignment), while parse-time issues (e.g., missing initializer) are SyntaxError.
