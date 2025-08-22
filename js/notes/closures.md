# JavaScript Closures

```
function x(){
    var a=7
    function y(){
        console.log(a)
    }
    return y;
}
```
```
function x(){
    var a=7
    return function y(){
        console.log(a)
    }
}
//same thing
```
we know when y is inside x it can access variable a due to lexical scope
so the console gives 7 when we execute function x and invoke y as well inside x only
since it is not available outside without return

let z=x();
z()
here our function x gets executed and as we know when we invoke a function 
a new execution context is created and pushed into callstack 
here it returns y so our y is stored in variable z and the execution context of function x is popped
basically the function x is gone
but the variable a defined inside function x can still be accessed by function y which is returned and stored in z 
this is called closure
#### A closure is a function that “remembers” the variables from its outer (enclosing) scope even after that scope has finished executing.
#### In short: Closure = function + its lexical environment

---

## uses

### 1. Private Variables (Data Encapsulation)
```
function createCounter() {
    let count = 0; // Private variable
    
    return {
      increment: () => ++count,
      decrement: () => --count,
      getCount: () => count
    };
}
  
const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount());  // 2

// count is not directly accessible
console.log(counter.count); // undefined
```

### 2. Function Generator / Currying
```
function multiplyBy(factor) {
    return function(number) {
      return number * factor;
    };
}
  
const double = multiplyBy(2);
const triple = multiplyBy(3);
  
console.log(double(5)); // 10
console.log(triple(5)); // 15
```

### 3. setTimeouts
```
function delayedGreeting(name) {
    let greeting = "Hello";

    setTimeout(function() {
        console.log(`${greeting}, ${name}!`);
    }, 1000);
}

delayedGreeting("Sujan"); 
// After 1 second, logs: "Hello, Sujan!"
// Inner function remembers 'greeting' and 'name' due to closure
```

