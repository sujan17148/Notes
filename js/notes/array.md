
# 🔹 1. What is an Array?

An **array** is an ordered collection of values (elements) stored in a single variable.

- Indexed (**0-based**).  
- Can hold any type: numbers, strings, objects, functions, or even mixed values.

```js
const arr = [1, "hello", true, { name: "JS" }];
```

---

# 📊 Array Methods Mutation Table

| **Mutates Original** | **Does NOT Mutate Original** |
|-----------------------|-------------------------------|
| push, pop, shift, unshift, splice, sort, reverse, fill, copyWithin | slice, concat, flat, flatMap, forEach, map, filter, some, every, reduce, reduceRight, find, findLast, findIndex, indexOf, lastIndexOf, includes, join, toString, Array.from, Array.of, Array.isArray, toSorted, toReversed |

---

# 🚀 Array Methods

### 1. `push(element1, element2, …)`
Adds elements to the **end** of array.

```js
let arr = [1, 2, 3];
arr.push(4, 5); // [1, 2, 3, 4, 5]
```

---

### 2. `pop()`
Removes and returns the **last element**.

```js
let arr = [1, 2, 3];
let last = arr.pop(); // last = 3, arr = [1, 2]
```

---

### 3. `unshift(element1, element2, …)`
Adds elements to the **beginning** of array.

```js
let arr = [2, 3];
arr.unshift(0, 1); // [0, 1, 2, 3]
```

---

### 4. `shift()`
Removes and returns the **first element**.

```js
let arr = [1, 2, 3];
let first = arr.shift(); // first = 1, arr = [2, 3]
```

---

### 5. `splice(startIndex, deleteCount, item1, item2, …)`
Removes/adds elements at specified position.

```js
let arr = [1, 2, 3, 4, 5];
arr.splice(2, 1, 'a', 'b'); 
// [1, 2, 'a', 'b', 4, 5]
// Removes 1 element at index 2, adds 'a', 'b'
```

---

### 6. `reverse()`
Reverses array order.

```js
let arr = [1, 2, 3];
arr.reverse(); // [3, 2, 1]
```

---

### 7. `sort(compareFunction)`
Sorts array elements.

```js
let arr = [3, 1, 4, 1, 5];
arr.sort((a, b) => a - b); // [1, 1, 3, 4, 5] ascending order
arr.sort((a, b) => b - a); // [5, 4, 3, 1, 1] descending order
```

---

### 8. `fill(value, startIndex, endIndex)`
Fills array with static value.

```js
let arr = [1, 2, 3, 4, 5];
arr.fill(0, 1, 3); // [1, 0, 0, 4, 5]
```

---

### 9. `concat(array1, array2, …)`
Merges arrays and returns a **new array**.

```js
let arr1 = [1, 2];
let arr2 = [3, 4];
let merged = arr1.concat(arr2); // [1, 2, 3, 4]

// Better approach (spread & rest)
let mergedAlt = [].concat(arr1, arr2); // [1, 2, 3, 4]
```

---

### 10. `slice(startIndex, endIndex)`
Extracts a section of array and returns a **new array**.

```js
let arr = [1, 2, 3, 4, 5];
let sliced = arr.slice(1, 4); // [2, 3, 4]
```

---

### 11. `join(separator)`
Joins array elements into a string.

```js
let arr = ['Hello', 'World'];
let str = arr.join(' '); // "Hello World"
```

---

### 12. `indexOf(searchElement, fromIndex)`
- Returns first index of element.  
- `fromIndex` optional. Negative values start counting from the end.  
- Returns `-1` if not found.  
- If item appears multiple times, returns first occurrence.

```js
let arr = [1, 2, 3, 2, 1];
let index = arr.indexOf(2); // 1
```

---

### 13. `lastIndexOf(searchElement, fromIndex)`
Same as `indexOf()`, but returns the **last occurrence**.

```js
let arr = [1, 2, 3, 2, 1];
let index = arr.lastIndexOf(2); // 3
```

---

### 14. `includes(searchElement, fromIndex)`
Checks if array contains element.  
- Can detect `NaN`.  
- Returns `true` if found, else `false`.

```js
let arr = [1, 2, 3];
let hasTwo = arr.includes(2); // true
```

---

### 15. `forEach(callback(element, index, array))`
- Executes function for each element.  
- Does **not** return anything.

```js
let arr = [1, 2, 3];
arr.forEach((item, index) => console.log(`${index}: ${item}`));
// 0: 1, 1: 2, 2: 3
```

---

### 16. `map(callback(element, index, array))`
Creates new array with transformed elements.

```js
let arr = [1, 2, 3];
let doubled = arr.map(x => x * 2); // [2, 4, 6]
```

---

### 17. `filter(callback(element, index, array))`
Creates new array with elements that pass test.

```js
let arr = [1, 2, 3, 4, 5];
let evens = arr.filter(x => x % 2 === 0); // [2, 4]
```

---

### 18. `reduce(callback(accumulator, element, index, array), initialValue)`
Reduces array to a single value.

```js
let arr = [1, 2, 3, 4];
let sum = arr.reduce((acc, curr) => acc + curr, 0); // 10
```

---

### 19. `find(callback(element, index, array))`
Returns first element that passes test.

```js
let arr = [1, 2, 3, 4, 5];
let found = arr.find(x => x > 3); // 4
```

---

### 20. `findLast(callback(element, index, array))`
Starts from the end of array and returns first element that passes test.  
Basically returns **last element** that passes test.

```js
let arr = [1, 2, 3, 4, 5];
let found = arr.findLast(x => x > 3); // 5
```

---

### 21. `toSorted(compareFunction)`
Returns a **new sorted array** (does not mutate original).

```js
let arr = [3, 1, 4, 1, 5];
arr.toSorted((a, b) => a - b); // [1, 1, 3, 4, 5] ascending order
arr.toSorted((a, b) => b - a); // [5, 4, 3, 1, 1] descending order
```

---

### 22. `toReversed()`
Returns a **new reversed array** (does not mutate original).

```js
let arr = [1, 2, 3];
let rev = arr.toReversed(); // [3, 2, 1]
console.log(arr); // [1, 2, 3] (original unchanged)
```

---

### 23. `findIndex(callback(element, index, array))`
Returns index of first element that passes test. 

```js
let arr = [1, 2, 3, 4, 5];
let index = arr.findIndex(x => x > 3); // 3
```

---

### 24. `some(callback(element, index, array))`
Tests if at least one element passes test.

```js
let arr = [1, 2, 3, 4, 5];
let hasEven = arr.some(x => x % 2 === 0); // true
```

---

### 25. `every(callback(element, index, array))`
- Tests if all elements pass test.  
- If one element fails, returns `false`.

```js
let arr = [2, 4, 6, 8];
let allEven = arr.every(x => x % 2 === 0); // true
```

---

### 26. `flat(depth)`
Flattens nested arrays.

```js
let arr = [1, [2, 3], [4, [5, 6]]];
let flattened = arr.flat(2); // [1, 2, 3, 4, 5, 6]
```

---

### 27. `flatMap(callback(element, index, array))`
Maps and flattens in one step.

```js
let arr = [1, 2, 3];
let result = arr.flatMap(x => [x, x * 2]); 
// [1, 2, 2, 4, 3, 6]
```