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


# Execution Context & Call Stack in JavaScript

## 📌 Problem  
Whenever a JavaScript program runs, a **Global Execution Context (GEC)** is created.  
- This GEC stays alive until the entire code has finished running.  
- Each time a function is invoked, a **new Execution Context (EC)** is created.  

👉 If we keep calling **nested functions**, we could end up with a **large number of ECs**.  
This might seem like it could cause issues (e.g., memory overload or confusion in execution order).

---


## ✅ Solution: The Call Stack  
JavaScript solves this problem using the **Call Stack**.  
- The **Global Execution Context** is pushed into the stack first.  
- Whenever a function is invoked, its **Execution Context** is pushed on top of the stack.  
- If that function has an inner function, its EC is also pushed on top.  
- Once a function finishes execution, its EC is **popped out** of the stack.  

This works like a **stack data structure (LIFO – Last In, First Out)**:  
- The most recent function pushed is the first one popped.  
- Once all functions are done, only the **Global EC** remains, and when the program ends, it’s also cleared.

---

## 🔑 Key Takeaways  
- **Execution Contexts** are created for global and function execution.  
- **Nested functions** create multiple ECs.  
- The **Call Stack** ensures smooth execution by managing them in **LIFO order**.  


# Execution Stack / Call Stack in Async JavaScript

JavaScript is **single-threaded**, so it can't "pause" for things like timers or network requests.  
Instead, async operations are sent outside the call stack to other environments (**Web APIs / Node.js APIs**) which can handle them in the background.

These environments use **two types of queues**:

1. **Micro-task queue**: for Promises, network requests  
2. **Macro-task queue**: for `setTimeout`, `setInterval`

---

## Working for `setTimeout`

- `setTimeout` is an anonymous function, so when invoked, it creates a **new execution context (EC)** which is **pushed into the call stack**.  
- Inside the EC:
  - It registers its **callback function** along with the **timer** into Web APIs.  
- The `setTimeout` EC is then **popped from the call stack**.  
- When the timer completes, the Web API / event loop **pushes the callback into the macro-task queue**.  
- When the **call stack is empty** (only the global EC remains):
  - The callback function’s **EC is created** and pushed onto the call stack.  
  - The callback executes and is **popped after execution**.  

✅ **Key point**: The callback’s EC is **created only when dequeued from the macro-task queue**, not when `setTimeout` is called.

---

## Working of a Promise

1. **Create a Promise**
```javascript
let p = Promise.resolve(data);
JS engine creates a Promise object in memory.
No EC is created for the callback yet.

Register .then() callback
```p.then(callback);```
The callback function is stored inside the Promise object.
Nothing runs yet; no EC is created.
Promise resolves
When the Promise is fulfilled or rejected, the callback is added to the micro-task queue.
Event loop checks call stack
If the call stack is empty (only the global EC remains):
The callback is dequeued from the micro-task queue.
A new EC for the callback is created and pushed onto the call stack.
The callback executes.
EC is popped after execution.

✅ Key point: Promise itself is JS-internal; only its callback runs later on the call stack via the micro-task queue.



# JavaScript Execution: S Async/Await
```async function foo() {
  await something();
  console.log("done");
}
```
async functions return a Promise automatically.
Calling foo() executes synchronously until the first await.
Pause at await
await waits for the Promise to resolve.
The rest of the function after await is scheduled as a micro-task.
Synchronous code outside the async function continues running.
Event loop execution
When the awaited Promise resolves:
Remaining code after await is added to micro-task queue.
Event loop checks call stack → EC is created for remaining code → pushed to stack → executes → popped.
