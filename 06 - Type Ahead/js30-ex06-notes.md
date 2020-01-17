# JS30 Ex06 Type Ahead Notes

### JS

1. To get data from some endpoint we use the `fetch` built in method of the Browser's API.
```js
fetch(endpoint) //returns a promise
  .then(blob => console.log(blob)) // blob is the raw data i.e. the response in the promise.
```
* Here, `fetch` returns a `Promise`(you can see it if you `console.log` it).
* When you do `.then` it returns the raw data, say a `blob` of data.

2. Now, what we want to do is convert that data into readable format. We know this particular data is in JSON format so we call `.json()` a method available on the blob (among others which you can see under `__proto__`). Then we convert that into an array using the `...` spread syntax, like so;
```js
const cities = [];
fetch(endpoint)
  .then(blog => blob.json())
  .then(data => cities.push(...data));
```
* Here we could have just pushed data items in `cities` but, since we want to maintain it's immutable nature, we push using the spread function, which pushed each item of `data` one by one into `cities`. If we had just pushed `data` itself, it's have pushed it as a single object so we'd end up with a 2D array since, every argument given to `push` adds that item as one whole.

3. Then we create a function `findMatches` to find get the filtered db w.r.t. our search query. We use regex and since the search query is a variable, we can't just pass in a variable inside regex syntax, like so; `match(/query/i)`. We have to create a separate regex variable and pass that to match, like so; `const re = new RegExp(query, 'gi')`
* **NOTE** when `console.log(findMatches('bos', cities));` it logged an empty array but, when called in the browser console itself, it gave the correct output of an array with 2 matches?

4. Now we want to capture the input. We select the element, `addEventListener` and call `displayMatches` on it.
* **NOTE** `change` event only is captured when you _complete_ the change i.e. here it'd mean after entering the query you come out of the element, say by clicking outside.
* We used `keyup` event since we want to display suggestions in real time.

5. `displayMatches` takes the query value (`this.value`). Then we return HTML elements with the matched values and pass it to the suggestions element of the document, like so;
```js
const html = matchesArray.map(place => {
  return (`
    <li>
      <span class="name">${place.city}, ${place.state}</span>
      <span class="population">${place.population}</span>
    </li>
    `).join('');
});
suggestions.innerHTML = html;
```
* Here, if we don't `join('')` then, it'll return an array of matches, and you'll see a `,` between each match(naturally). Hence, by joining we convert all the matches into a string.

6. Now we need to highlight the matching query in realtime so we apply the `hl` class to the matching query by replacing `this.value`(the query, again since `displayMatches` is called by the event listener on the `queryInput`). like so;
```js
const cityHl = place.city.replace(re, `<span class="hl">${this.value}</span>`);
```
And then pass `cityHl` instead of `place.city` in the return.

7. For adding the commas, there's a standard function available in the community `numbersWithCommas` and applied it to `place.population`.

## NOTE

The `displayMatches` in is what React exists for, to make doing such things easy and powerful.


