# Why Do We Need Promises in JavaScript?

## Callback Hell / Pyramid of Doom

Consider a scenario where multiple asynchronous tasks depend on each other:

```js
createOrder(userId, function() {
    updateCart(userId, function() {
        updateWallet(userId, function() {
            createOrder(userId, function() {
                processPayment(userId, function() {
                    sendConfirmation(userId, function() {
                        console.log("Order flow complete!");
                    });
                });
            });
        });
    });
});
```

Here:
- The **cart can only be updated** after creating an order.
- The **wallet can only be updated** after updating the cart.  

This makes asynchronous tasks dependent on each other.  

While async operations are helpful, nesting callbacks like this quickly becomes:
- Hard to read  
- Hard to maintain  
- Error-prone  

This deep nesting is called the **“pyramid of doom.”**

---

## Inversion of Control

When using callbacks, we **pass control** to the asynchronous function (e.g., `createOrder`) and rely on it to call the next function (`updateCart`) at the right time.

👉 If `createOrder` forgets to call `updateCart`, the flow breaks.  

This **loss of control** is known as **inversion of control.**

---

## One Workaround: Extracting Callbacks

We can define the callbacks outside and just pass them as parameters:

```js
function handlePayment() {
    sendConfirmation(userId, function() {
        console.log("Order flow complete!");
    });
}

function handleWallet() {
    createOrder(userId, handlePayment);
}

function handleCart() {
    updateWallet(userId, handleWallet);
}

createOrder(userId, handleCart);
```

### Problem with Scoping

If your callback relies on variables from the outer function where it was defined, moving it outside **loses access to those variables.**

Example:

```js
function createOrderFlow(userId) {
    let orderId = "ORDER123"; // only available inside this scope

    function handlePayment() {
        console.log(orderId); // works here
    }

    updateCart(userId, handlePayment);
}
```

If you move `handlePayment` **outside** `createOrderFlow`, it won’t have access to `orderId` anymore, because `orderId` is part of the lexical scope of `createOrderFlow`.

---

## Solution: Promises

Promises solve both **callback hell** and **inversion of control**.

They allow chaining asynchronous operations in a linear, readable way using **`.then()`**, or even more elegantly using **`async/await`**.

👉 With promises, the code becomes easier to **manage, maintain, and reason about**, while keeping the dependencies intact.

---

## What is a Promise?

A **Promise** is an object that represents the **future result** of an asynchronous operation.  

It’s like a placeholder for a value that isn’t available yet, but will be either:

- **Resolved (fulfilled)** → success  
- **Rejected** → failure  

Think of it as a “promise” to deliver a result later.

### Promise States:
1. **Pending** – The async operation hasn’t finished yet.  
2. **Fulfilled** – The operation completed successfully, producing a value.  
3. **Rejected** – The operation failed, producing a reason (error).  

### Example:

```js
const myPromise = new Promise((resolve, reject) => {
    // some async operation
    setTimeout(() => {
        const success = true; 
        if (success) resolve("Task completed!");  // fulfilled
        else reject("Task failed!");              // rejected
    }, 1000);
});

myPromise
    .then(result => console.log(result))  // runs if resolved
    .catch(error => console.error(error)); // runs if rejected
```
Perfect 👍 Let’s complete the picture by comparing **`Promise.any()`** and **`Promise.race()`**, alongside `Promise.all()` and `Promise.allSettled()`.  

---

 ## Comparing JavaScript Promise Combinators

JavaScript provides four main combinators for handling multiple promises:  
- `Promise.all()`  
- `Promise.allSettled()`  
- `Promise.race()`  
- `Promise.any()`  

---

## 1. `Promise.all()`

- Waits for **all promises to resolve**.  
- If **any promise rejects**, the whole operation fails immediately (fail-fast).  
- Returns an **array of resolved values**.  

✅ Use when you **need all promises to succeed**.

```js
Promise.all([
  Promise.resolve(1),
  Promise.resolve(2),
  Promise.resolve(3)
]).then(console.log); 
// [1, 2, 3]
```

```js
Promise.all([
  Promise.resolve(1),
  Promise.reject("Error"),
  Promise.resolve(3)
]).catch(console.log);
// "Error"
```

---

## 2. `Promise.allSettled()`

- Waits for **all promises to settle** (resolve or reject).  
- **Never rejects**.  
- Returns an array of **objects**:  
  - `{ status: "fulfilled", value: ... }`  
  - `{ status: "rejected", reason: ... }`  

✅ Use when you want **all results, regardless of success or failure**.

```js
Promise.allSettled([
  Promise.resolve(1),
  Promise.reject("Error"),
  Promise.resolve(3)
]).then(console.log);
/*
[
  { status: "fulfilled", value: 1 },
  { status: "rejected", reason: "Error" },
  { status: "fulfilled", value: 3 }
]
*/
```

---

## 3. `Promise.race()`

- Resolves or rejects as soon as **the first promise settles**.  
- Doesn’t care about the outcome of the other promises.  

✅ Use when you only care about **the fastest result** (first success or first failure).

```js
Promise.race([
  new Promise(resolve => setTimeout(() => resolve("First"), 1000)),
  new Promise(resolve => setTimeout(() => resolve("Second"), 2000)),
  new Promise(resolve => setTimeout(() => resolve("Third"), 3000))
]).then(console.log);
// "First"
```

If the fastest promise rejects:

```js
Promise.race([
  Promise.reject("Fail fast"),
  new Promise(resolve => setTimeout(() => resolve("Slow success"), 2000))
]).catch(console.log);
// "Fail fast"
```

---

## 4. `Promise.any()`

- Resolves as soon as **the first promise fulfills**.  
- Ignores rejections, waits until it finds a **successful result**.  
- If **all promises reject**, it throws an **`AggregateError`**.  

✅ Use when you want **the first successful result**, regardless of failures.

```js
Promise.any([
  Promise.reject("Error 1"),
  new Promise(resolve => setTimeout(() => resolve("Success!"), 1000)),
  Promise.reject("Error 2")
]).then(console.log);
// "Success!"
```

If all fail:

```js
Promise.any([
  Promise.reject("Error 1"),
  Promise.reject("Error 2")
]).catch(console.log);
// AggregateError: All promises were rejected
```

---

## 📊 Summary Table

| Method              | Resolves When                           | Rejects When                               | Return Value                     | Use Case |
|----------------------|------------------------------------------|--------------------------------------------|----------------------------------|----------|
| **`Promise.all()`** | All promises fulfill                     | Any promise rejects (**fail fast**)         | Array of values                   | Need all results to succeed |
| **`Promise.allSettled()`** | All promises settle                  | Never                                       | Array of `{status, value/reason}` | Need results of all, even failures |
| **`Promise.race()`** | First promise settles (resolve/reject)  | First promise rejects (if it happens first) | Value/reason of first settled     | Need the fastest result (success or fail) |
| **`Promise.any()`** | First promise fulfills                   | All promises reject                         | Value of first fulfilled promise  | Need the first success |

---

👉 Together:
- `all` → all succeed or fail fast.  
- `allSettled` → wait for everything, success + failure results.  
- `race` → first to settle wins (success or fail).  
- `any` → first success wins, ignores failures.  

---

# Async/Await 


## Understanding Multiple `await` with the Same or Different Promises

## 1. Reusing the Same Promise

```js
const myPromise = new Promise((resolve, reject) => {
    // some async operation
    setTimeout(() => {
       resolve("Task completed!");
    }, 10000);
});

async function solvePromise() {
    const data = await myPromise;
    console.log(data);

    const data1 = await myPromise;
    console.log(data1);
}
```

### Explanation
- The promise `myPromise` starts executing immediately when it’s created.  
- It takes **10 seconds** to resolve the first time.  
- On the first `await`, the function pauses until the promise is resolved.  
- On the second `await`, the promise is **already resolved**, so it does **not wait another 10 seconds**.  

✅ Result: Both logs happen almost back-to-back (not separated by 10 seconds).

---

## 2. Waiting on Two Different Promises

```js
const p1 = new Promise(resolve => setTimeout(() => resolve("p1 done"), 10000));
const p2 = new Promise(resolve => setTimeout(() => resolve("p2 done"), 5000));

async function solvePromise() {
    console.log("working");
    const data = await p1;
    console.log(data);
    const data1 = await p2;
    console.log(data1);
}

solvePromise();
```

### Explanation

- `p1` and `p2` are created immediately and their timers start running independently.  
- When `solvePromise` executes:
  - `"working"` is logged immediately.  
  - Execution suspends at `await p1`.  
  - After **10 seconds**, `p1` resolves and `"p1 done"` is logged.  
  - Now execution suspends at `await p2`.  
  - Since `p2` was scheduled **5 seconds earlier**, it has already resolved.  
  - `"p2 done"` is logged immediately.  

This gives the **illusion** that both promises are resolved simultaneously, but actually:
- `p1` resolves after 10 seconds.  
- `p2` resolves after 5 seconds (before `p1` finishes).  

The internal working is the same—the timers run independently of `async/await`.

---

## Real Asynchronous Operation Example

```js
async function getData() {
    const p = fetch("https://jsonplaceholder.typicode.com/todos/1"); // network request starts immediately
    const response = await p; // pauses here until network responds
    const data = await response.json(); // pauses until JSON is parsed
    console.log(data);
}

getData();
```

### How it Works

- When `getData()` is invoked, it enters the call stack.  
- At the first `await`, execution **suspends** and `getData`’s context is popped out of the stack.  
- Meanwhile, the **network request** runs in the background.  
- Once the request resolves (fulfilled/rejected), the `getData` context is pushed back into the call stack and execution continues.  
- The same process happens with the second `await` for JSON parsing.  

👉 The behavior is the same with timers (`setTimeout`) too, but timers are **independent of `await`**, so they just resolve after their scheduled time.

---

✅ All content preserved, but now structured clearly with headings, code blocks, explanations, and flow.  

Would you like me to also **add diagrams (ASCII or Markdown-style flow)** for **callback hell vs promises vs async/await** to make it even more intuitive?