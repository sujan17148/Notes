# 🌀 Debouncing in JavaScript

**Debouncing** is a technique to limit how often a function runs.  
Instead of firing on every event (like `keyup`, `scroll`, or `resize`), the function waits for a **pause in activity**.

### 🔎 Logic
When an event triggers repeatedly, reset a timer.  
Only if no new event happens during the wait time, the function executes.

### ✅ Why?
- Prevents unnecessary calls  
- Improves performance (especially for input search, window resize, scroll handlers)

**Key point**: Debounce ensures a function runs **once after the last event** in a burst.

```js
function debounce(func, delay) {
  let timer;
  return function(...args) {
    clearTimeout(timer); 
    timer = setTimeout(() => func.apply(this, args), delay);
  };
}

// Example usage:
const search = (query) => {
  console.log("Searching for:", query);
};

const debouncedSearch = debounce(search, 500);

document.getElementById("searchBox")
  .addEventListener("input", (e) => debouncedSearch(e.target.value));
```

---

# ⏳ Throttling in JavaScript

**Throttling** is a technique to control how often a function runs.  
Instead of firing on every event, it ensures the function executes **at most once in a fixed interval** (even if events keep coming).

### 🔎 Logic
When an event triggers repeatedly:
- Run the function immediately.
- Ignore extra calls until the delay passes.

### ✅ Why?
- Prevents function overload on frequent events  
- Keeps UI smooth (great for scroll, resize, mousemove)

**Key point**: Throttle ensures a function runs **at regular intervals** during a burst.

```js
function throttle(func, delay) {
  let last = 0;
  return function(...args) {
    const now = Date.now();
    if (now - last >= delay) {
      func.apply(this, args);
      last = now;
    }
  };
}

const logScroll = () => console.log("Scrolled!");

const throttledScroll = throttle(logScroll, 1000);

window.addEventListener("scroll", throttledScroll);
```

---

# ⚡ Debounce vs Throttle in JavaScript

## 🌀 Debounce
- Waits until **no events** happen for a set delay  
- Executes **only once** after the burst of events  

**Best for:**
- Search box input  
- Resize events  
- Auto-saving drafts  

**Timeline (events = `|`, execution = ✅)**  
```
Events:   | | |   |     | | 
Debounce:             ✅     ✅
```

---

## ⏳ Throttle
- Executes function at **regular intervals**, ignoring extra calls  
- Ensures execution happens **at least once every delay period**  

**Best for:**
- Scroll handlers  
- Mouse movement tracking  
- Infinite scrolling  

**Timeline (events = `|`, execution = ✅)**  
```
Events:   | | |   |     | | 
Throttle: ✅     ✅     ✅
```

---

## 👉 Core Difference
- **Debounce** → *“Do it after I stop firing events.”*  
- **Throttle** → *“Do it every X ms, no matter what.”*  


# Event Delegation

Event delegation is a JavaScript technique where instead of attaching event listeners to multiple child elements, you attach a single event listener to their parent element and use event bubbling to catch events from the children.

## 🔑 Why use event delegation?

- **Performance** → Instead of attaching dozens/hundreds of listeners, you only need one.
- **Dynamic elements** → Works for elements added later (e.g., new items in a list).
- **Cleaner code** → Less repetition.

```
// Adding individual listeners to each button
const buttons = document.querySelectorAll('.item-button');
buttons.forEach(button => {
    button.addEventListener('click', function(e) {
        console.log('Button clicked:', this.textContent);
    }, true or false); 
    // false is default (bubbling child to parent) 
    // when true it captures event (from parent to child)
});

// Event Delegation Approach
const container = document.getElementById('button-container');
container.addEventListener('click', function(e) {
    // Check if clicked element is a button
    if (e.target.classList.contains('item-button')) {
        console.log('Button clicked:', e.target.textContent);
    }
});
```

## 🔑 Summary

- **Capturing**: parent → child (top-down).
- **Bubbling**: child → parent (bottom-up).
- Default is bubbling, but you can reverse (capture) with `{ capture: true }`.