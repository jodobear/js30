# JS30 ex07 Array Cardio 2

* **Arrow functions**: You can only implicitly return if you pass an expression! And when implicitly returning always put your expression in parenthesis!

* `array.find()` returns the first item that matches the query.

## Deleting

Two ways to mutably delete:
```js
// 1. using the operator delete
delete comments[findIndexOf]

// 2. using splice()
comments.splice(findIndexOf, 1);
```
Preferred (even in redux) and immutable way of deleting:
```js
const newComments = [
...comments.slice(0, findIndexOf),
...comments.slice(findIndexOf + 1)
]
```
Notice, we are slicing the array here not `splice`. This way you create a new copy and your original stays as is. If you don't provide the second argument then it'll go till the end.
