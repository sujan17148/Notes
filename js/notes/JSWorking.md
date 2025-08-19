# How JavaScript Works

- Everything in JS happens **inside an Execution Context**.  
- JavaScript is a **synchronous, single-threaded language**.  
  - 🔹 It can only execute **one command at a time**.  
  - 🔹 Being synchronous, JS executes the next line **only after the current line finishes**.

---

video refrence: [https://www.youtube.com/watch?v=iLWTnMzWtj4&list=PLlasXeu85E9cQ32gLCvAvr9vNaUccPVNP&index=3](https://www.youtube.com/watch?v=iLWTnMzWtj4&list=PLlasXeu85E9cQ32gLCvAvr9vNaUccPVNP&index=3)

---

# Execution Context (EC) in JavaScript

An **Execution Context** is an **abstract concept** (an idea) that holds the **environment in which JavaScript code is evaluated and executed**.  
*(This means the execution context stores everything JS needs to run your code.)*  

> 💡 **Basically, it’s a container that keeps all the information JS needs to run a piece of code.**

---

## Components of Execution Context

### 1️⃣ Environment Variables (Memory Block)
- Stores **variables and their values**, **functions**, as key-value pairs.

**JS engine allocates memory for:**
- **Variables:**  
  - `var` → initialized with `undefined`  
  - `let` & `const` → in **Temporal Dead Zone (TDZ)** until assignment  
- **Functions:** Fully hoisted with their definitions  
- **Scope chain:** Determines variable accessibility  
- **`this` binding:** Determines the value of `this` in context  

> 📝 Think of it as a **memory block where memory is reserved** before code runs.

---

### 2️⃣ Thread of Execution (Code Block)
- JS engine executes the code **line by line**.
- Values are assigned to variables, functions are invoked, and expressions are evaluated.

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
1️⃣ Creation Phase (Memory Block)
JS engine scans the code before execution and reserves memory:
Memory Block:
a → undefined      // var is hoisted with undefined
b → TDZ             // let is in Temporal Dead Zone
sum → function(x, y){ return x + y; }  // function fully hoisted
sumResult-> TDZ
this → global object
```
### 2️⃣ Execution Phase (Thread of Execution / Code Block)
```
JS executes **line by line**:
// Global Execution Context starts

// Line 1
console.log(a);
// a exists in memory, value is undefined → prints undefined

// Line 2
var a = 10;
// Value 10 is assigned → memory now a → 10

// Line 3
let b = 20;
// b leaves TDZ, value 20 is assigned → memory now b → 20

// Line 4
// Function sum is already in memory → nothing changes

// Line 5
const sumResult = sum(a, b, 30);
// Calls the function sum(a, b, 30)
// 🔹 This creates a **new Function Execution Context** inside the main/global context

/* Function Execution Context (for sum) */

// Creation Phase (Memory Block)
let x = undefined;
let y = undefined;
let z = 30;        // passed value
var result = undefined; // due to var

// Execution Phase
x = 10;
y = 20;
result = x + y + z; // result = 60
return result;      // 🔹 returns value to sumResult and context shifts back to global

// 🔹 also the whole execution context for sum function is deleted and if invoked again a new context is made

console.log(sumResult); // prints 60

// 🔹 once the whole code has been executed the whole global execution context also gets deleted

```
#Memory Block:
```
a → 10
b → 20
sum → function(x, y){ return x + y; }
this → global object
```

#for functions
| Function Type             | var / let / const | Creation Phase | Execution Phase |
| ------------------------- | ----------------- | -------------- | --------------- |
| Function Declaration      | N/A               | Fully hoisted  | Already usable  |
| Function Expression       | var               | undefined      | Assigned value  |
| Arrow Function Expression | let / const       | TDZ            | Assigned value  |
