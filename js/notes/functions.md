```markdown
# 🔥 JavaScript Functions — Complete Guide

---

## ✅ What is a Function?

A **function** is a reusable block of code designed to perform a particular task.  
Functions are executed when they are **called (invoked).**

```js
// Function declaration
function greet(name) {
  return "Hello, " + name + "!";
}

console.log(greet("Alice")); // "Hello, Alice!"
console.log(greet("Bob"));   // "Hello, Bob!"
```

```js
// Using template literals
function greet(name) {
    return `Hello, ${name}!`;
}

console.log(greet("Alice")); // "Hello, Alice!"
```

---

## 🔹 Types of Functions in JavaScript

---

### 1. Function Declaration (Named Function)

```js
// Function Declaration - Hoisted
function square(number) {
  return number * number;
}

console.log(square(5)); // ✅ Works even before declaration (due to hoisting)
```

**Characteristics:**
- ✅ Fully hoisted → Can be called before declaration  
- ✅ Named → Always has a function name  
- ✅ Block-scoped in strict mode  

---

### 2. Function Expression

```js
// Function Expression - Not hoisted
const square = function (number) {
  return number * number;
};

// Named Function Expression
const factorial = function fac(n) {
  return n < 2 ? 1 : n * fac(n - 1);
};
```

**Characteristics:**
- ❌ Not hoisted → Cannot be called before declaration  
- Can be **anonymous** or **named**  
- Often assigned to variables  

---

### 3. Arrow Function (ES6+)

Arrow functions provide **shorter syntax** and a **different `this` binding**.  

```js
// Simple arrow function
const add = (a, b) => a + b;

// Multiple parameters
const multiply = (x, y) => x * y;

// Single parameter (no parentheses needed)
const double = x => x * 2;

// No parameters
const sayHello = () => "Hello!";

// Block body (explicit return required)
const calculate = (a, b) => {
    const sum = a + b;
    return sum * 2;
};
```

#### Arrow Function Characteristics
```js
// 1. No 'this' binding
const obj = {
    name: "Alice",
    regularFunction: function() {
        console.log(this.name); // "Alice"
    },
    arrowFunction: () => {
        console.log(this.name); // undefined (window.name in browsers)
    }
};

// 2. Cannot be used as constructors
const MyFunction = () => {};
// new MyFunction(); // ❌ TypeError

// 3. No 'arguments' object
const regularFunc = function() {
    console.log(arguments); // Arguments object
};

const arrowFunc = (...args) => {
    console.log(args); // ✅ Works (array)
};
```

✅ **Good for:**
- Callbacks
- Array methods (`map`, `filter`, `reduce`)
- Short inline functions  

❌ **Avoid for:**
- Object methods
- Event handlers (when `this` is needed)
- Functions requiring `arguments` object
- Constructors  

---

### 4. Anonymous Function

Functions **without a name**, often used in callbacks.

```js
setTimeout(function () {
  console.log("I run after 2 seconds");
}, 2000);
```

---

### 5. Immediately Invoked Function Expression (IIFE)

Executes **immediately** after being defined.  
Useful for **private scopes**.

```js
(function () {
  console.log("IIFE executed immediately!");
})();

// With parameters
(function (name) {
  console.log(`Hello, ${name}!`);
})("World");

// Returning values
const result = (function (a, b) {
  return a + b;
})(5, 3);
console.log(result); // 8
```

---

### 6. Higher-Order Functions

Functions that **take other functions as arguments** or **return functions**.

```js
// Takes a function as argument
function processArray(arr, callback) {
  const result = [];
  for (let item of arr) {
    result.push(callback(item));
  }
  return result;
}

const numbers = [1, 2, 3, 4, 5];
const doubled = processArray(numbers, (x) => x * 2);

// Returns a function
function createValidator(type) {
  return function (value) {
    switch (type) {
      case "email": return /\S+@\S+\.\S+/.test(value);
      case "phone": return /\d{10}/.test(value);
      default: return true;
    }
  };
}

const emailValidator = createValidator("email");
console.log(emailValidator("test@example.com")); // true
```

---

### 7. Recursive Function

A function that **calls itself**.

```js
function factorial(n) {
  if (n === 0) return 1;
  return n * factorial(n - 1);
}

console.log(factorial(5)); // 120
```

---

## 🔹 Arguments Handling

### 1. The `arguments` Object (Traditional Functions Only)

```js
function oldStyleSum() {
  let total = 0;
  for (let i = 0; i < arguments.length; i++) {
    total += arguments[i];
  }
  return total;
}

console.log(oldStyleSum(1, 2, 3, 4)); // 10
```

---

### 2. Rest Parameters (Arrow & Modern Functions)

```js
const arrowStyleSum = (...args) => {
  let total = 0;
  for (let i = 0; i < args.length; i++) {
    total += args[i];
  }
  return total;
};

console.log(arrowStyleSum(1, 2, 3, 4)); // 10
```

---

# 🎯 Summary

- **Function Declaration** → Hoisted, always named  
- **Function Expression** → Not hoisted, can be anonymous  
- **Arrow Functions** → Shorter syntax, no `this`, no `arguments`, not constructors  
- **Anonymous Functions** → Often used in callbacks  
- **IIFE** → Executes immediately, creates private scope  
- **Higher-Order Functions** → Take/return other functions  
- **Recursive Functions** → Call themselves  
- **Arguments vs Rest** → `arguments` (old), `...rest` (modern & better)

---
```