# JS30 ex04 Array Cardio 1

Given a set of exercises on sort, filter, map & reduce. Pretty straight forward exercises.

* Even though i do not call a function, if there's a `console.log()` inside a function, it get's printed in the console.

## ex01: `filter`

`Array.prototype.filter()` filters collection based on condition.

Syntax: `object.filter(item => return item if meets condition)`

## ex02: `map`

`Array.prototype.map()` maps a certain function to the items in collection.
Syntax: `object.map(item => // do something to each item)`

## ex03: `sort`

`Array.prototype.sort()` sorts the collection _in-place_ using a compare function:

```js
function compare(a, b) {
  if (a is less than b by some ordering criterion) return -1;
  if (a is greater than b by the ordering criterion) return 1;
  // a must be equal to b
  return 0;
}
```
Usage:
```js
function ascendingSorted = array.sort(a, b) {
  a - b;
  // alt:
  a > b ? 1 : -1
}
```
The way this works is it recursively compares each element with it's next one and keeps ordering the items based on their return values until the sorted order bubbles out.

I don't think this is computationally efficient, seems like the bubble_sort implementation.

## ex04: `reduce`

`Array.prototype.reduce()` reduces the contents of an object to a value based on the function.

Syntax:
```js
object.reduce((accumulator, currentItem) => {
  // do something with current item;
  const result = processedCurrentItem;
  return accumulator += result;
  }, {});
```
Very powerful method. Anywhere you need a count you can use this you just have to figure out how.

## ex05: `sort`

Here we had to sort inventors by years lived. So, we made a function `life` to get the years lived value and compared those values in `sort`.

## ex06: `de` Boulevards of Paris

This was an interesting exercise. We performed in the devConsole on the wikipedia page. One line solution:
```js
const de = [...document.querySelectorAll('.mw-category a')]
              .map(link => link.textContent)
              .filter(boulevard => boulevard.includes('de'));
```
This solution highlights chaining functions. Following new things:

* In the following code notice we can call DOM methods even on objects, they don't necessarily have to be elements.
```js
// First we get the element containing all the links
    const container = document.querySelector('.mw-category');
    // then we extract the element that contain the links and convert it to an array.
    const links = Array.from(container.querySelectorAll('a'));
```

* `...` the _spread_: syntax sugar to unpack an object. It will take every item of the iterable object and put them into the containing array.

* `.textContent` extracts text from the specified node(element) and __all its descendants__. There is a `nodValue` property that returns the value (text) only of the specified node. Similar to `innerText` property except:
  * `innerText` returns the content of all elements, except for `<script>` and `<style>` elements.
  * `innerText` won't return text of elements hidden by CSS while `textContent` will.
  * __TIP:__ it set or return HTML content of an element use `innerHTML` property.

## ex07: `sort`

* Here i learned to destructure an object's contents straight into an array, like so;

`const [aLast, aFirst] = a.split(', '); // a = 'Blake, William' gives [''Blake', 'William'];`

* Why does `sort` do this?
```js
const lastSort = people.sort((cur, next) => {
  console.log(cur)  // this prints the whole list
  console.log(next) // this prints the list EXCEPT the last element.
})
```

## ex08: `reduce`

Here we got in to exploring the flexibility of `reduce`.

* First thing of note is the second argument of `reduce`;

`object.reduce((acc, currVal) => {// do something}, {});`

That `{}` is an object. In the previous example we set it to `0` since it as a simple counting job, but in this we had to count instances of all distinct items in `data`. So, we populated that object based on keys of all distinct items in the collection and set their values to `0`. Then each time we encounter the same key we increment its count.