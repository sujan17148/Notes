
## 1. what is output of the give code ?

```javascript
function add(a, b) {
    console.log(a + b);
}
console.log(add(2, 3)); // 5
let result = add(2, 3);
console.log(result);    // undefined
```

👉 The result is `undefined` because the function does not explicitly return anything, it only logs to the console.

---

## 2. Function Declaration vs Function Expression

### Function Declaration (Hoisted)
```javascript
console.log(add(2, 3)); // Works! Output: 5

function add(a, b) {
    return a + b;
}
```

### Function Expression (Not Hoisted)
```javascript
console.log(multiply(2, 3)); // ❌ Error: Cannot access 'multiply' before initialization

const multiply = function(a, b) {
    return a * b;
};
```

**Key Differences**:
- **Hoisting**: Declarations are hoisted, expressions are not.
- **Naming**: Declarations must have a name, expressions can be anonymous.
- **Scope**: In strict mode, declarations are block-scoped.

---

## 3. Function Hoisting

```javascript
// ✅ Works due to hoisting
console.log(hoistedFunction()); // "I'm hoisted!"

function hoistedFunction() {
    return "I'm hoisted!";
}

// ❌ Doesn't work
console.log(notHoisted()); // TypeError
console.log(notHoisted);   // undefined

var notHoisted = function() {
    return "I'm not hoisted!";
};

// ❌ With let/const → Temporal Dead Zone
console.log(alsoNotHoisted()); // ReferenceError

const alsoNotHoisted = function() {
    return "I'm in TDZ!";
};
```

---

## 4. Pure vs Impure Functions

### ✅ Pure Function
```javascript
function add(a, b) {
  return a + b;
}

console.log(add(2, 3)); // 5
console.log(add(2, 3)); // Always 5
```

### ❌ Impure Function
```javascript
let count = 0;

function increment() {
  count++;
  return count;
}

console.log(increment()); // 1
console.log(increment()); // 2
```

👉 **Fun Fact**: `Math.random()` and `Date.now()` are impure since they give different outputs with the same input.

---

## 5. Arrow Functions vs Regular Functions

```javascript
// Regular Function
function regularFunction(name) {
    return `Hello, ${name}!`;
}

// Arrow Function
const arrowFunction = (name) => `Hello, ${name}!`;
```

### Differences
1. **`this` binding**
```javascript
const obj = {
    name: 'Alice',
    regularMethod: function() {
        console.log(this.name); // Alice
    },
    arrowMethod: () => {
        console.log(this.name); // undefined
    }
};
```

2. **Arguments object**
```javascript
function regularFunc() {
    console.log(arguments); // works
}

const arrowFunc = () => {
    console.log(arguments); // ❌ ReferenceError
};
```

3. **Constructor usage**
```javascript
function RegularConstructor() {
    this.name = 'Regular';
}
const regular = new RegularConstructor(); // ✅ Works

const ArrowConstructor = () => {
    this.name = 'Arrow';
};
const arrow = new ArrowConstructor(); // ❌ TypeError
```

---

## 6. Higher-Order Functions

```javascript
// Takes a function as argument
function processArray(arr, callback) {
    const result = [];
    for (let i = 0; i < arr.length; i++) {
        result.push(callback(arr[i], i));
    }
    return result;
}

const numbers = [1, 2, 3, 4, 5];
console.log(processArray(numbers, num => num * 2));
// [2, 4, 6, 8, 10]

// Returns a function
function createMultiplier(multiplier) {
    return function(number) {
        return number * multiplier;
    };
}

const double = createMultiplier(2);
console.log(double(5)); // 10
```

### Built-in Examples
```javascript
const fruits = ['apple', 'banana', 'cherry'];

const uppercased = fruits.map(fruit => fruit.toUpperCase());
const longNames = fruits.filter(fruit => fruit.length > 5);
const totalLength = fruits.reduce((sum, fruit) => sum + fruit.length, 0);
```

---

## 7. Difference between arguments object and rest parameters

### Arguments Object
```javascript
function traditionalFunction() {
    console.log(arguments);             // array-like
    console.log(Array.isArray(arguments)); // false
    console.log(arguments.callee);      // reference to function
    console.log(arguments.length);      // number of args
}
```

### Rest Parameters
```javascript
function modernFunction(...args) {
    console.log(args);              // real array
    console.log(Array.isArray(args)); // true
}
```

👉 Arrow functions **don’t** have `arguments`, but can use rest `(...args)`.

---

## 8. Difference betweeen function Call vs Constructor Call

```javascript
function Person(name) {
    this.name = name;
}

const person1 = Person('Alice');   // No 'new'
const person2 = new Person('Bob'); // With 'new'

console.log(person1); // undefined
console.log(person2); // Person { name: 'Bob' }
console.log(window.name); // "Alice"
```

### Without `new`
- `this` → global object (`window` in browsers).
- Updates `window.name = "Alice"`.
- Function returns `undefined`.

### With `new`
- Creates a new empty object.
- Links it to `Person.prototype`.
- Binds `this` to the new object.
- Returns the object (`Person { name: 'Bob' }`).

---