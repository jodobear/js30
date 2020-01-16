# JS30 EX03 CSS Variables

The web page provided in this exercise displays an image, and has 3 form inputs from which the user can manipulate the padding, blur amount, and background color of the image. Update the CSS and write the JavaScript code necessary to bring functionality to the inputs.

## Guide

The purpose of this exercise is to gain experience using CSS3 variables. These are different from Sass-style variables; Sass variables are defined in the Sass file, but once compiled to CSS the values cannot be updated. CSS3 variables can have their values updated using JavaScript. The `input` HTML elements have a `name` property that corresponds with the CSS properties we want to update. We can create CSS3 variable references and attach them to the `root` element, provide them with some default values, and utilize JavaScript to attach event listeners to the `input` HTML elements that will call upon an event handler function whenever the `input` `values` have been changed by the user. We will define the function to target the entire document and update the values of the CSS variables from there.

## Steps:

### CSS:

* Declare a new style for the `:root` element and declare three variables inside the style definition for `:root` with the same names as the `input` HTML elements. CSS3 variables are declared in the following syntax format:
```css
/* Two hyphens (--) followed by the variable name */
/* note the variable names are the same as `name` prop of the element */
:root {
  --base: #ffc600;
  --blur: 10px;
  --padding: 10px;
}
```
* Declare a new style for the `img` element and set the `background`, `filter`, and `padding` properties to the variables we defined at the root element:
```css
/* 'var(--variableName)' to use previously defined CSS properties */
img {
  background: var(--base);
  filter: blur(var(--blur));
  padding: var(--padding);
}
```
* Declare a new style for the `.hl` class and set the `color` to the `base` variable.

## JavaScript:

* Declare & define a variable as a reference to all of the `inputs` on the page, `document.querySelectorAll('selector')` returns a `NodeList`.
  * `NodeList` isn't an array as it has less methods available compared to an array like `map`, `reduce` etc. though it has `forEach`.

* Iterate through the HTML Node Elements that the variable is referencing and attach event listeners to each one that will call on an event handler whenever the `input` `value` has been changed (the `change` event).

* Repeat step 2, listening for mouse movements on the inputs instead of value changes (the `mousemove` event).

* Define a function that will be used as the event handler. It will update the `value` of the CSS3 variable at the root document level corresponding with the name property of the input element which called this function.
  * Minor 'gotcha': Properties like `padding` and `blur` won't update because the value from the input does not include the type of measurement we are using (`px`, `em`, etc.). The `input` HTML elements also have a `data-sizing` property if they require a suffix. We can use this to attach the correct suffix to the value if necessary.

## Learnings

### HTML

* `for` property of `<label>` tag binds that label to the `name` property of the element passed as value to it.
* `<input type='range'>` adds a scroll bar to the page with  `min`, `max` properties and `value` as default value.
* `<input type='color'>` adds a color picker to the page with `value` as it's default value.

### CSS

* `:root` selector for the base HTML element.

* `<h2 style="--base: x">Update CSS Variables with <span class="hl">JS</span></h2>` When you do this it'll override the `--base: color` property that's set to root as it occurs after(inside) root.

### JS

* `const suffix = this.dataset.sizing || '';` the `|| ''` means return _nothing_ instead of `undefined` since we don't have a suffix for `color`.
* `document.documentElement` returns the root element.
* ```document.documentElement.style.setProperty(`--${this.name}`, this.value + suffix);``` updates the CSS property of the first arg with the value supplied in the second arg.
* Interesting difference in function definition syntax:
```js
// legacy
function handleUpdate(e) {
  console.log(this.value);
}
// arrow
const handleUpdate = (e) => {
  console.log(this.value);
}
```
When defined as arrow func, it returns `undefined` even if explicitly returned but, when defined using legacy syntax, returns the `value`. This is because `console.log(this.value);` is a statement not an expression. Arrow functions need an expression to implicitly return.

* `mousemove` is another event like `transitionend` event in last exercise.
