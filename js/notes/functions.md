## Functions

### What is a Function?

🔹 What is a Function?

👉 In JavaScript, a function is a block of code designed to perform a task.
It is reusable, meaning you can call it multiple times without rewriting the code.

```js
//function delcaration
function greet(name) {
  return "Hello, " + name + "!";
}

console.log(greet("Alice")); // "Hello, Alice!"
console.log(greet("Bob")); // "Hello, Bob!"
```

### Types of Functions in JavaScript

✅ 1. Function Declaration (Named Function)

The “classic” way of writing functions.

Fully hoisted (you can call it before it’s defined).

```js
// Function Declaration
function greet(name) {
  return "Hello, " + name;
}

console.log(greet("Alice")); // "Hello, Alice"
```

✅ 2. Function Expression

Function stored inside a variable.

Not hoisted (must be defined before use).

Can be named or anonymous.

✅ 3. Arrow Function (ES6+)

Shorter syntax.

Does not have its own this (important in OOP & callbacks).

Great for small, inline functions and callbacks

```js
const greet = (name) => "Hello, " + name;

console.log(greet("Charlie")); // "Hello, Charlie"
```

✅ 4. Anonymous Function

Functions without a name.

Often used in callbacks (like in setTimeout, event listeners).

```js
setTimeout(function () {
  console.log("I run after 2 seconds");
}, 2000);
```

✅ 5. Immediately Invoked Function Expression (IIFE)

Runs as soon as it is defined.

Used to create a private scope.

```js
(function () {
  console.log("I run immediately!");
})();
```

✅ 6. Higher-Order Function

A function that takes another function as an argument or returns a function.

```js
function operate(a, b, fn) {
  return fn(a, b);
}

console.log(operate(5, 3, (x, y) => x + y)); // 8
```

✅ 7. Recursive Function

A function that calls itself.

```js
function factorial(n) {
  if (n === 0) return 1;
  return n * factorial(n - 1);
}

console.log(factorial(5)); // 120
```
