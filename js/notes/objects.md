# 📘 JavaScript Objects 

## 1. Object Definition
An **object** is a collection of **key–value pairs** (called **properties**).

- **Keys** → usually strings (or symbols).  
- **Values** → any type (primitive, array, object, function, etc.).

```js
let person = {
  name: "Alice",
  age: 25,  
  isStudent: false
};
```

👉 Objects are the backbone of JavaScript (everything from arrays to functions are objects).

---

## 2. Accessing Properties

### 🔹 Dot notation (simple keys only)
```js
person.name; // "Alice"
```

### 🔹 Bracket notation (for dynamic/special keys)
```js
let key = "age";
person["name"]; //Alice
person[key]; // 25
```

✅ **Tip:** Use **dot notation** when the key is known and a valid identifier.  
✅ Use **bracket notation** when the key is dynamic or contains special characters/spaces.

```js
let obj = { "first-name": "Alice" };
obj["first-name"]; // ✅ Works
obj.first-name;    // ❌ Error
```

---

## 3. Add, Modify, Delete

```js
// Add
person.city = "London";

// Modify
person.age = 30;

// Delete
delete person.isStudent;
```

⚠️ **Note:** `delete` only removes own properties. It does not affect prototype properties.

---

## 4. Checking Property Existence

### 🔍 `hasOwnProperty()`
Checks if an **object itself** has a property (not inherited from prototype).

```js
person.hasOwnProperty("name");      // true
person.hasOwnProperty("toString");  // false (inherited from Object.prototype)
```

### 🔍 `Object.hasOwn(obj, prop)` (ES2022+)
A **modern, safer replacement** for `hasOwnProperty`.

```js
Object.hasOwn(person, "name");     // true
Object.hasOwn(person, "toString"); // false
```

### 🔍 `in` operator
Checks both **own and inherited properties**.

```js
"name" in person;     // true
"toString" in person; // true (inherited)
```

⚡ **Summary of Property Checks**
- `obj.hasOwnProperty(key)` → traditional, but may fail if `obj` shadows the method.  
- `Object.hasOwn(obj, key)` → ✅ modern, recommended.  
- `key in obj` → true if property exists (own or inherited).  

---

## 5. Useful Built-in Object Methods

### 🔑 Keys / Values / Entries
```js
Object.keys(person);   // ["name", "age", "city"]
Object.values(person); // ["Alice", 30, "London"]
Object.entries(person);// [["name","Alice"], ["age",30], ["city","London"]]
```

👉 Commonly used with loops:
```js
for (let [key, value] of Object.entries(person)) {
  console.log(`${key}: ${value}`);
}
```

---

### 🔄 Merge & Copy
```js
let a = { x: 1 }, b = { y: 2 };

let merged = Object.assign({}, a, b); // {x:1, y:2}
let spread = { ...a, ...b };          // {x:1, y:2}
```

- `Object.assign()` → copies properties into target object.  
- Spread `...` → shorter, more modern syntax.  

⚠️ **Note:** Both perform **shallow copy**. Nested objects are still shared.

---

### ❄️ Freeze & Seal
```js
let frozen = Object.freeze({ pi: 3.14 });
frozen.pi = 4; // ❌ ignored (read-only)

let sealed = Object.seal({ name: "Bob" });
sealed.age = 30;          // ❌ can't add
sealed.name = "Charlie";  // ✅ can modify
```

- `Object.freeze(obj)` → prevents **adding, removing, modifying** properties.  
- `Object.seal(obj)` → prevents **adding/removing**, but allows modification of existing properties.  

---

### 🔄 From Entries
```js
Object.fromEntries([["a",1],["b",2]]);
// {a:1, b:2}
```

👉 Opposite of `Object.entries()` → converts key–value pairs back into an object.

---

## ⚡ Quick Recap
- Objects store **key–value pairs**.  
- Access with **dot** or **bracket notation**.  
- Modify with assignment, delete with `delete`.  
- Check properties with:
  - `Object.hasOwn(obj, key)` → ✅ best way  
  - `key in obj` → includes inherited props  
- Useful helpers: `keys`, `values`, `entries`, `assign`, spread, `freeze`, `seal`, `fromEntries`.  
