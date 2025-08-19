1: what is the output?
```
function add(a,b){
    console.log(a+b)
}
console.log(add(2,3)) // 5
let result=add(2,3)
console.log(result)
```
 gives undefined since we just assgined function execution to result but the function itself does not return anything explictly so it returns undefined by default.
