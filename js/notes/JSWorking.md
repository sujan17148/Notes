# How JavaScript Works

* Everything in JS happens **inside an Execution Context**.
* JavaScript is a **synchronous, single-threaded language**.

  * 🔹 It can only execute **one command at a time**.
  * 🔹 Being synchronous, JS executes the next line **only after the current line finishes**.

---

video reference: [https://www.youtube.com/watch?v=iLWTnMzWtj4\&list=PLlasXeu85E9cQ32gLCvAvr9vNaUccPVNP\&index=3](https://www.youtube.com/watch?v=iLWTnMzWtj4&list=PLlasXeu85E9cQ32gLCvAvr9vNaUccPVNP&index=3)

---

# Execution Context (EC) in JavaScript

An **Execution Context** is an **abstract concept** (an idea) that holds the **environment in which JavaScript code is evaluated and executed**.
*(This means the execution context stores everything JS needs to run your code.)*

> 💡 **Basically, it’s a container that keeps all the information JS needs to run a piece of code.**

---

## Components of Execution Context

### 1️⃣ Environment Variables (Memory Block)

* Stores **variables and their values**, **functions**, as key-value pairs.

**JS engine allocates memory for:**

* **Variables:**

  * `var` → initialized with `undefined`
  * `let` & `const` → in **Temporal Dead Zone (TDZ)** until assignment
* **Functions:** Fully hoisted with their definitions
* **Scope chain:** Determines variable accessibility
* **`this` binding:** Determines the value of `this` in context

> 📝 Think of it as a **memory block where memory is reserved** before code runs.

---

### 2️⃣ Thread of Execution (Code Block)

* JS engine executes the code **line by line**.
* Values are assigned to variables, functions are invoked, and expressions are evaluated.

> 📝 Think of it as a **thread of execution that runs instructions from the memory block**.

---

# Example: Creation & Execution Flow

```javascript
console.log(a); // Line 1
var a = 10;     // Line 2
let b = 20;     // Line 3

function sum(x, y, z) { // Line 4
  var result = x + y + z;
  return result;
}

const sumResult = sum(a, b, 30);
console.log(result); // Line 5
```

---

### 1️⃣ Creation Phase (Memory Block)

JS engine scans the code **before execution** and reserves memory:

```text
Memory Block (Global EC):
a → undefined        // var is hoisted
b → TDZ               // let in Temporal Dead Zone
sum → function(x, y, z) { ... } // function fully hoisted
sumResult → TDZ
this → global object
```

---

### 2️⃣ Execution Phase (Thread of Execution / Code Block)

JS executes **line by line**:

```javascript
// Line 1
console.log(a); // prints undefined (var hoisted)

// Line 2
var a = 10;     // memory now a → 10

// Line 3
let b = 20;     // leaves TDZ, memory now b → 20

// Line 4
// function sum already in memory → nothing changes

// Line 5
const sumResult = sum(a, b, 30);
// 🔹 Creates a new Function Execution Context (for sum)

  // Function EC Creation Phase
  let x = undefined;
  let y = undefined;
  let z = 30;
  var result = undefined;

  // Function EC Execution Phase
  x = 10;
  y = 20;
  result = x + y + z; // result = 60
  return result;      // 🔹 returns value to sumResult, context shifts back to global

// 🔹 The execution context for sum function is deleted after it finishes
console.log(sumResult); // prints 60

// 🔹 Once the whole code has executed, the global execution context is also deleted
```

---

### Memory Block After Execution

```text
Global Memory Block:
a → 10
b → 20
sum → function(x, y, z) { ... }
sumResult → 60
this → global object
```

---

### Functions & Hoisting

| Function Type             | var / let / const | Creation Phase | Execution Phase |
| ------------------------- | ----------------- | -------------- | --------------- |
| Function Declaration      | N/A               | Fully hoisted  | Already usable  |
| Function Expression       | var               | undefined      | Assigned value  |
| Arrow Function Expression | let / const       | TDZ            | Assigned value  |
