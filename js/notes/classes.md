# Classes in JavaScript

In JavaScript, classes don’t exist in the true sense like in OOP languages such as C++ or Java.  
Classes are syntactic sugar over constructor functions. They provide a clean, organized way to create objects and handle inheritance.  

A class defines a blueprint for objects, including their properties and methods.

---

## Class Declaration

```js
class Person {
  constructor(name, age) {  // constructor method
    this.name = name;
    this.age = age;
  }

  // Method
  greet() {
    console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);
  }
}

// Creating an instance
const person1 = new Person("Sujan", 20);
person1.greet(); // Hello, my name is Sujan and I am 20 years old.
```

- `constructor` is a special method called automatically when creating a new object with `new`.  
- Methods inside classes don’t need the `function` keyword.  
- `this` inside class methods points to the instance.  

---

## Class Expressions

You can also define classes as expressions:

```js
const Animal = class {
  constructor(type) {
    this.type = type;
  }

  speak() {
    console.log(`${this.type} makes a sound`);
  }
};

const dog = new Animal("Dog");
dog.speak(); // Dog makes a sound
```
--- 

# What `instanceof` Does

`instanceof` checks if an object is created by a particular class (constructor function) or is in its prototype chain.

```js
class Animal {}
class Dog extends Animal {}

const d = new Dog();

console.log(d instanceof Dog);    // true
console.log(d instanceof Animal); // true (because Dog extends Animal)
console.log(d instanceof Object); // true (all objects inherit from Object)
```

---

## Static Methods

Static methods belong to the class itself, not to instances.

```js
class MathUtils {
  static add(a, b) {
    return a + b;
  }
}

console.log(MathUtils.add(5, 7)); // 12
// const mu = new MathUtils();
// mu.add(5, 7); // ❌ Error: add is not a function
```

---

## Inheritance

Classes can extend other classes to inherit properties and methods.

```js
class Person {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  greet() {
    console.log(`Hello, I'm ${this.name}`);
  }
}

class Student extends Person {
  constructor(name, email, grade) {
    super(name, email); // calling parent constructor
    // We must call super before using "this"
    // We need to set properties of parent before adding custom properties
    // If we don’t need parent variables just call super(), it is still required
    this.grade = grade; // child-specific property
  }

  study() {
    console.log(`${this.name} is studying in grade ${this.grade}`);
  }
}

const student1 = new Student("Sujan", "sujan@gmail.com", 12);
console.log(student1.name);  // Sujan
console.log(student1.email); // sujan@gmail.com
student1.study();            // Sujan is studying in grade 12
```

**Note:** You must call `super()` before using `this` in the child constructor. Otherwise, JS will throw a `ReferenceError`.  
`super()` essentially runs the parent constructor, initializing all properties defined there.

---

## Using `super` to Call Parent Methods

```js
class Person {
  greet() {
    console.log("Hello from Person");
  }
}

class Student extends Person {
  greet() {
    super.greet(); // call parent method
    // this.greet(); also works 
    // super.greet() is used to override parent method like when we have same method name as parent
    //  since its just a console so any will be fine here
    console.log("Hello from Student");
  }
}

const s = new Student();
s.greet();
// Output:
// Hello from Person
// Hello from Student
```
```
class Student extends Person {
  getName() {
    return `Student: ${super.getName()}`; // calls parent method
  }
}

const s = new Student("Sujan");
console.log(s.getName()); // Student: Sujan
```

---

## Private and Public Fields

- **Public:** normal properties accessible outside.  
- **Private:** start with `#` and accessible only inside the class.  

```js
class BankAccount {
  #balance = 0; // private

  deposit(amount) {
    this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }
}

const account = new BankAccount();
account.deposit(500);
console.log(account.getBalance()); // 500
// console.log(account.#balance); // ❌ Error
console.log(account.balance); // undefined
```
### Accessing Private Fields in Inheritance

- Private fields of a parent **cannot** be accessed directly by a child class.  
- To interact with them, the child must use parent methods.  
- This can be done using:
  - `this.parentMethod()` → calls the parent’s method if not overridden.  
  - `super.parentMethod()` → calls the parent’s method explicitly when overridden in the child.  



### Getters and Setters in JavaScript

**Getters** and **Setters** are special methods that allow you to access and modify properties of an object in a controlled way.

- **Getter (`get`)**: Allows you to access a property like a variable, but behind the scenes, it executes a function.
- **Setter (`set`)**: Allows you to assign a value to a property like a variable, but behind the scenes, it executes a function.

### Common Uses
1. **Encapsulation**: Control how a property is read or modified.
2. **Computed Properties**: Calculate a value when accessed.
3. **Validation**: Check values before setting them.

### Example

```js
class User {
  constructor(name, age) {
    this._name = name;  // convention: underscore for "private-like" properties
    this._age = age;
  }

  // Getter for 'age'
  get age() {
    return this._age;
  }

  // Setter for 'age'
  set age(value) {
    if (value < 0) {
      console.log("Age cannot be negative!");
      return;
    }
    this._age = value;
  }
}

const user1 = new User("Alice", 20);

// Using getter
console.log(user1.age);  // 👉 20 (calls the get age())

// Using setter
user1.age = 25;          // 👉 sets _age to 25 via set age()
console.log(user1.age);  // 👉 25

user1.age = -5;          // 👉 Age cannot be negative!
console.log(user1.age);  // 👉 25 (unchanged)
```

### Important Notes:
- You **don’t call getters and setters like functions**. Instead of `user1.age()`, you just use `user1.age`. or for setter `user1.age=5`
- **Getters must return something**.
- **Setters must accept exactly one parameter**.
- You can have:
  - **A getter without a setter** (read-only property).
  - **A setter without a getter** (write-only property, though this is rare).

Let me know if you'd like more examples or details!