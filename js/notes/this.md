# this keyword

### 1. **Default Behavior (Global Context)**

- **In Browsers:**  
  In the global scope of a browser, `this` refers to the `window` object. This means you can access properties and methods of the `window` object via `this`, like `this.alert()`.

- **In Node.js:**  
  The `this` keyword in the global context is the `global` object in Node.js, which is different from the browser environment.

### 2. **`this` Substitution Rule**

- **Non-strict Mode:**  
  When `this` is `undefined` or `null` in non-strict mode, it defaults to the global object (in the browser, this is `window`). This can sometimes lead to unexpected behavior, especially if you're trying to use `this` in functions expecting it to be something else.

- **Strict Mode:**  
  In strict mode, `this` does **not** default to the global object. Instead, if `this` is `undefined` or `null`, it stays `undefined`. This behavior helps avoid bugs that arise from implicitly referencing the global object.

```javascript
"use strict";
function example() {
  console.log(this);  // undefined, because in strict mode, `this` is not automatically replaced.
}
example();
```

### 3. **Function Calls**

- **In Non-Strict Mode:**  
  When you call a function in non-strict mode, `this` will refer to the global object (`window` in browsers). However, if the function is part of an object, `this` will refer to that object.
  
- **In Strict Mode:**  
  In strict mode, calling a function globally (without attaching it to an object) results in `this` being `undefined`. This is safer because you avoid accidental global variable creation.

```javascript
function test() {
  console.log(this);
}

test();  // In non-strict mode, logs the global object (window in browsers).
```

- **When Called as a Method:**

```javascript
const obj = {
  test: function() {
    console.log(this);
  }
};

obj.test(); // `this` refers to `obj`
```

- **`window.x()` in the browser:**  
  Even if strict mode is on, calling a function directly from the global context like `window.x()` will still bind `this` to the `window` object. This can be tricky since strict mode is supposed to avoid this type of behavior.

### 4. **Inside DOM Event Handlers**

When you use `this` inside a DOM event handler (such as an inline `onclick` attribute), it refers to the DOM element that triggered the event.

Example:

```html
<button onclick="console.log(this)">Click me</button>
```

In this case, `this` inside the `onclick` handler will refer to the `button` element itself, so you can access its properties like `this.tagName` or `this.value` directly.

---

### 🔑 **Key Rule (Revisited):**
The value of `this` depends entirely on the **call-site** (how the function is invoked), **not** where the function is defined.

- **Global context** → `window` in browsers (or `global` in Node.js).
- **Method call** → `this` refers to the object the method is called on.
- **Constructor functions** (using `new`) → `this` refers to the newly created instance of the object.
- **Event handlers in the DOM** → `this` refers to the element that triggered the event.
