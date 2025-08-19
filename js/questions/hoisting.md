## Hoisting & Scope Questions

1. Explain hoisting with an example.

Hoisting moves declarations to the top of their scope:

```js
console.log(x); // undefined (not error)
var x = 5;
```

// JavaScript interprets it as:

```js
var x;
console.log(x); // undefined
x = 5;
```

2.What will this code output?

```js
var a = 1;
function test() {
  console.log(a);
  var a = 2;
}
test();
```

`undefined` - due to hoisting, the local `var a` declaration is moved to the top of the function, shadowing the global variable.

4. What's the difference between function declarations and function expressions?

Function Declaration: Fully hoisted, can be called before definition

Function Expression: Not hoisted, treated as variable assignment

```js
// Works
sayHello();
function sayHello() {
  console.log("Hello");
}
```

```js
// Doesn't work
sayBye(); // TypeError
var sayBye = function () {
  console.log("Bye");
};
```

5. Predict the output:

```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
```

ans: it prints 3 three times because variable i is declared with var and since its functionally scoped not blocked by the time console inside timeout is executed the value of i is 3 and it will be shared by each console

if we had used let or const here then since these are block scoped each timeout would had their own unique i so it would print 0 1 2

here one more thing var print 3 3 3 not 2 2 2 since our i<3 but then also we have 3 how?

7. What will this complex hoisting example output?

```js
var x = 1;
function foo() {
  console.log(x);
  if (false) {
    var x = 2;
  }
}
foo();
```

`undefined` - Even though the `if` block never executes, `var x` is hoisted to the top of the function, creating a local variable that shadows the global `x`.

8. Explain the scope chain with this example:

```js
var global = "global";
function outer() {
  var outerVar = "outer";
  function inner() {
    var innerVar = "inner";
    console.log(global + outerVar + innerVar);
  }
  inner();
}
outer();
```

JavaScript looks for variables in this order: 1. Local scope (`innerVar`) 2. Outer function scope (`outerVar`) 3. Global scope (`global`)
This creates a "scope chain" that inner functions traverse to find variables.
