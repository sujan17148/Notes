# Window Object in JavaScript

## 1. What is the Window Object?

- It is created automatically by the browser when a web page is loaded.
- It represents the current browser window or tab.
- Every global variable or function you create in JavaScript inside the browser becomes a property or method of the window object.
- The `this` keyword points to the window object in global execution context.

```js
var a = 10;
console.log(window.a); // 10
console.log(this.a); // 10
console.log(a); // 10
```

In browsers, the Global Execution Context (GEC) is stored inside the window object.
That means all global scope variables and functions live inside window.

```js
function greet() {
  return "Hello";
}
console.log(window.greet()); // Hello
console.log(this.greet()); // Hello
console.log(greet()); // Hello
```
But let and const variables are stored in a different Script scope, not on the global window object.
 Example:

```js
let x = 10;
console.log(window.x); // undefined
console.log(x); // 10
```

## 2. Global Execution Context Structure


```
Global Execution Context
 ├── Memory (Variable Environment)
 │     ├── var → attached to window
 │     ├── function → attached to window
 │     └── let/const → in Script scope (not on window)
 └── Code (Execution)
```

```js
var x = 1;
let y = 2;

console.log(this.x); // 1 (in global scope `this` === window)
console.log(this.y); // undefined here since this points to window object
console.log(window.y); // undefined since y is in different script scope not in window object
console.log(y); //2
console.log(script.y) // 2 
// since y is in script object and it gives error if the value is not initialized unlike var which is stored in window global object
```

## 3. Important Properties of Window

### 📍 Browser Info

- **`window.navigator`** → info about browser & OS.
  - It has objects like clipboard, geolocations, storage
  - Variables like vendor (about browser), platform (OS)

```js
// Browser information
console.log(navigator.userAgent); // Browser user agent string
console.log(navigator.platform); // Operating system platform
console.log(navigator.language); // Browser language
console.log(navigator.cookieEnabled); // Whether cookies are enabled
console.log(navigator.onLine); // Whether browser is online

// Browser capabilities
console.log(navigator.geolocation); // Geolocation API
console.log(navigator.clipboard); // Clipboard API
console.log(navigator.storage); // Storage API
```

- **`window.location`** → current URL & allows navigation.

```js
// Current URL information
console.log(location.href); // Complete URL
console.log(location.protocol); // Protocol (http:, https:)
console.log(location.hostname); // Domain name
console.log(location.port); // Port number
console.log(location.pathname); // Path after domain
console.log(location.search); // Query string
console.log(location.hash); // Fragment identifier

// Navigation methods
location.reload(); // Reload current page
location.assign("https://example.com"); // Navigate to new URL
location.replace("https://example.com"); // Replace current page
location.href = "https://example.com"; // Navigate to new URL
```

- **`window.history`** → browser history navigation.
  - Can move back or forward in browser history with methods like forward, backward

```js
// History navigation
history.back(); // Go back one page
history.forward(); // Go forward one page
history.go(-2); // Go back 2 pages
history.go(1); // Go forward 1 page

// History information
console.log(history.length); // Number of pages in history
console.log(history.state); // Current history state

// Push new state to history
history.pushState({ page: 1 }, "Page 1", "?page=1");
```

### 📍 Dimensions

- **`window.innerWidth`**, **`window.innerHeight`** → size of viewport.
- **`window.outerWidth`**, **`window.outerHeight`** → size including browser chrome.

### 📍 Document & DOM

- **`window.document`** → gives access to the DOM (document object).

### 📍 Timing

- **`setTimeout()`**, **`setInterval()`**, **`clearTimeout()`**, **`clearInterval()`**.

### 📍 Storage

- **`window.localStorage`**, **`window.sessionStorage`**.

### 📍 Dialog Boxes

- **`alert()`**, **`confirm()`**, **`prompt()`**.
