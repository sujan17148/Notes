## Scope and Hoisting

### Scope

🔍 What is Scope?

Scope determines where variables can be accessed in your code. Think of it like different rooms in a house - some things are accessible everywhere (living room), some only in specific rooms (bedroom closet).

📍 Types of Scope

Global Scope

Variables declared outside any function or block are globally accessible:

```js
let globalVariable = "I'm global";

function test() {
  console.log(globalVariable); // Accessible everywhere
}
```

Function Scope

Variables declared inside functions are only accessible within that function:

```js
function myFunction() {
  var functionScoped = "Only accessible here";
  console.log(functionScoped); // Works
}
// console.log(functionScoped); // ReferenceError
```

Block Scope

Variables declared with let and const inside {} are block-scoped:

these were introduced in es6+ along with let and const keyword for variable declaration

```js
if (true) {
  let blockScoped = "Only in this block";
  const alsoBlockScoped = "Me too!";
}
// console.log(blockScoped); // ReferenceError
```

### var vs let vs const

var vs let vs const

all three are keyword for variable declaration

|            | var                                                                   | let                                                                                                                             | const                                                                                                                             |
| ---------- | --------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| About      | it is oldest way of variable delcaration and rapidely used before es6 | it was introduced in es+                                                                                                        | it was also introduced in es+                                                                                                     |
| Redeclare? | variable delcared with var keyword can be redclared                   | variables delcared with let keyword cannot be redeclared                                                                        | variables delcared with const keyword cannot be redefined or redclared                                                            |
| Reassigned?  | can be reassigned                                                      | can be reassigned                                                                                                                | cannot be reassigned                                                                                                              |
| Scope      | these are functionally scoped                                         | these are functionally + blocked scoped                                                                                         | these are functionally + blocked scoped                                                                                           |
| Hoisting   | var delcaration are hoisted but initialized with undefined            | let declaration are hoisted but not initialized they stay in temporal dead zone which gives ReferenceError if tried to accessed | const declaration are hoisted but not initialized they stay in temporal dead zone which gives ReferenceError if tried to accessed |

Original notes:

var vs let vs const

all three are keyword for variable declaration

var: it is oldest way of variable delcaration and rapidely used before es6
variable delcared with var keyword can be redclared + redefined
these are functionally scoped
var delcaration are hoisted but initialized with undefined

let: it was introduced in es+
variables delcared with let keyword cannot be redeclared but can be redefined
these are functionally + blocked scoped
let declaration are hoisted but not initialized they stay in temporal dead zone which gives ReferenceError if tried to accessed

const: it was also introduced in es+
variables delcared with const keyword cannot be redefined or redclared
these are functionally + blocked scoped
const declaration are hoisted but not initialized they stay in temporal dead zone which gives ReferenceError if tried to accessed

### Hoisting

Hoisting:Hoisting is JavaScript's behavior of moving declarations to the top of their scope during compilation.

Variable Hoisting:

var declarations are hoisted but initialized as undefined

let and const are hoisted but not initialized (Temporal Dead Zone)

```js
console.log(hoistedVar); // undefined (not error)
var hoistedVar = "Hello";
```

```js
console.log(hoistedLet); // ReferenceError
let hoistedLet = "World";
```

Function Hoisting:

Function declarations are fully hoisted

Function expressions are not hoisted

```js
// Works - function declaration hoisted
sayHello(); // "Hello!"

function sayHello() {
  console.log("Hello!");
}
```

```js
// Doesn't work - function expression not hoisted
sayBye(); // TypeError
var sayBye = function () {
  console.log("Bye!");
};
```

### Variable Shadowing

Variable Shadowing: When a variable in an inner scope has the same name as one in an outer scope, the inner one "shadows" the outer.

```js
let x = 10;
function demo() {
  let x = 20; // Shadows global x
  console.log(x); // 20
}
demo();
console.log(x); // 10
```

⚠️ Shadowing is allowed with let/const but illegal if it creates conflict with var inside same scope type.

```js
function test() {
  var x = 10;
  let x = 20; // ❌ SyntaxError: Identifier 'x' has already been declared
}
```

👉 Rule of thumb:

Shadowing is fine if variables belong to different scopes.

Shadowing is illegal if var tries to shadow let or const in the same function/block scope.

### Lexical Scope (Static Scope)

Lexical scope means a function’s scope is determined by where it is written(defined), not by where it is called.

```js
function outer() {
  let outerVar = "Outer";

  function inner() {
    console.log(outerVar); // ✅ Has access (lexical scope)
  }
  return inner;
}

let fn = outer();
fn(); // "Outer"
```

The inner function remembers the environment it was created in, even when called outside.

Basis of closures in JavaScript.
