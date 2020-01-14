# JS30 Day01: Drum Kit

## What

An HTML page displays a collection of `div` elements, each containing a letter that corresponds with a key on the keyboard, and the name of the soundclip to be played when that button is clicked. When a user presses a key that matches one of the letters displayed in the `div` elements, the page should play the corresponding soundclip and place a temporary 'highlight' (or border) around the `div`. Write the JavaScript code necessary to add this functionality.

## Guide

We are provided with the HTML, CSS, and sound clips necessary to create this page/application. Let's go over the provided files and look at the pieces that we can utilize to fulfill the requirements:

1. HTML `data-*` attributes: Introduced in HTML5, `data-*` attributes (where `*` can be anything you want) allow us to store custom data on any HTML element. Each `div.key` (`<div class="key" data-key="...">`) and audio element in the provided HTML file has a `data-key` attribute which corresponds with a keyboard button.

2. CSS `playing` class & pre-defined style: The provided CSS file already has a `playing` class defined with some rules in it. We will apply this class to the correct element, depending on the key pressed by the user, and remove it once animation is finished.

And that's...pretty much all we should need from the HTML & CSS files. We can use the `data-key` attributes to match up the correct audio clip with the `div` element, and we can use the `playing` class to add that temporary highlight/border.

## Steps:

1. Create a function to play sound with the `event` argument. In that;
  1. Define two variables to select the relevant key press using either `div` or class `.key` as `querySelector` and audio using `tag[prop="event.keyCode"]`.
  2. Add the `playing` style only to the `key`/`audio` that is encoded.
  3. play the `audio` and set audio's `currentTime` to `0` before every play so it plays at every `keydown` event.
2. Add an `eventListener` to the `window` to listen for the `keydown` event and pass the function `playSound`.
3. Now that we've got the audio working and the `playing` style with yellow border, we need to remove that after we are done playing. For that we get all the keys of the `document` and loop over each key, add an `eventListener` to listen for `transitionend` and pass the `removeTransition` function to it.
4. For the `removeTransition` function we pass the `event` as it's argument to remove the `playing` style of the element where we applied it, which will be referred to with `this` as `removeTransition` is called by the `eventListener` and called upon the `key`. Note that we only check if the event's `propertyName` is `transform` since, it takes the most time to transform and we want to remove the transition after that's finished.

## Learnings

### JS

* [keycodes.info](https://keycode.info) - handy website to get keycodes for keys on the keyboard.

#### STEP 01
* `window`: The window object represents an open window in a browser. The Window interface represents a window containing a DOM document; the document property points to the DOM document loaded in that window. If a document contains frames (`<iframe>` tags), the browser creates one window object for the HTML document, and one additional window object for each frame.
* `addEventListener(event, function(return value of the event) {...})`
* `keydown`: is a defined term meaning when you press a key on the keyboard. It returns an object full of data.

#### STEP 02
* The `data-key` attribute value corresponds to the `keyCode` attribute of the object returned by `keydown` event. You can access it just like any attribute of an object; `event.keyCode`.
* `document.querySelector()`: The Document method `querySelector()` returns the first Element within the document that matches the specified selector, or group of selectors. If no matches are found, `null` is returned.
* ``audio[data-key="${e.keyCode}"]`` syntax for a query selector (similar to CSS) with a formatted variable using template strings syntax `${e.keyCode}`. Note the `""` surrounding the attribute value.
* `audio.currentTime = number` will set the current time of the audio to the number.
* `key.classList.add('playing');` $=$ key.addClass('playing') in jQuery.
* Now we linked the animation to keypress but, it is persistent, it needs to go away too.

#### STEP 03, 04, 05

* We get all the items with class key from the document and iterate through them and check for when the transition ends. We can't use `keys.addEventListener('transitionend')` since it can only listen to one event at a time, like so;
```js
  keys.forEach(key => addEventListener('transitionend', removeTransition));
```
Then we created the function removeTransition:
```js
function removeTransition(e) {
    console.log(e);
}
```
Which returned all the events that transitioned when pressed the button; `box-shadow`, `border-bottom-color`, `border-top-color`, `border-right-color`, `border-left-color` and `transform`. But, we are only interested in the event with `propertyName`: `transform`. Which we selected, like so;
```js
function removeTransition(e) {
    if (!e.propertyName == 'transform') return;
}
```
This eliminated all the returns we were getting.
* `this`: always == whatever got called against it.
```js
function removeTransition(e) {
    if (e.propertyName !== 'transform') return; //skip if not transform.
    this.classList.remove('playing');
}
```
Here `this` =
```html
<div data-key="65" class="key">
    <kbd>A</kbd>
    <span class="sound">clap</span>
</div>
```
As `this` was called inside `removeTransition` which was called by the `addEventListener` against the `key` in the `forEach` function and `removeTransition` will remove class `playing` from `this`.

And *viola*!

### HTML
* In the code all the `div` tags and all the `audio` tags have `data-key` attribute. This links them together.
* `<kbd>`: The HTML Keyboard Input Element <kbd> represents user input and produces an inline element displayed in the browser's default monospace font.

* `data-*='string'` tag attribute. The `*` can be anything - always lowercase. If they contain `-`, they will be converted to camelCase. Can be used with the HTML `dataset` API:
```js
let xyz = htmlTag.dataset.*
// htmlTag is the tag where you have your data-* attribute & * is whatever * is, accessed through dataset method.
```
* `<audio></audio>` embed sound content in documents. It may contain one or more audio sources, represented using the src attribute or the `<source>` element: the browser will choose the most suitable one. It can also be the destination for streamed media, using a `MediaStream`. Example:
```html
<figure>
    <figcaption>Listen to the T-Rex:</figcaption>
    <audio
        controls
        src="/media/examples/t-rex-roar.mp3">
            Your browser does not support the
            <code>audio</code> element.
    </audio>
</figure>
```

### CSS

* `rem`: A relative unit, like `em`, but it is always relative to the "root" element (i.e. `:root {}`) rather than using the cascade like `em` does. This vastly simplifies working with relative units. Better to use `rem` than `em`. Check this article on [css-tricks.com](https://css-tricks.com/theres-more-to-the-css-rem-unit-than-font-sizing/).
* [Length of CSS objects](https://css-tricks.com/the-lengths-of-css/) - article about all the lengths in CSS.
* `padding: length breadth;` internal margin, within the element.
* `border: length style color;` border styling.
* `border-radius: 1.5rem;` border edges. Higher the number rounder the border. At one point it'll become a circle.
* `margin: 1rem;`: margin between elements.
* `transition: all .07s ease;` The transition property is a shorthand property used to represent up to four transition-related longhand properties:
```css
.example {
    transition: [transition-property] [transition-duration] [transition-timing-function] [transition-delay];
}
```
These transition properties allow elements to change values over a specified duration, animating the property changes, rather than having them occur immediately. Example that transitions background color of a <div> element on :hover:
```css
div {
  transition: background-color 0.5s ease;
  background-color: red;
}
div:hover {
  background-color: green;
}
```
Check this article for further details; [css-tricks.com](https://css-tricks.com/almanac/properties/t/transition/)
* `width: 10rem;` The width CSS property sets an element's width. By default, it sets the width of the content area, but if box-sizing is set to border-box, it sets the width of the border area. The `min-width` and `max-width` properties override width.
* `text-shadow: 0 0 .5rem black;` Adds shadows to text. It accepts a comma-separated list of shadows to be applied to the text and any of its decorations. Each shadow is described by some combination of X and Y offsets from the element, blur radius, and color. Syntax:
```css
/* offset-x | offset-y | blur-radius | color */
text-shadow: 1px 1px 2px black;

/* color | offset-x | offset-y | blur-radius */
text-shadow: #fc0 1px 0 10px;

/* offset-x | offset-y | color */
text-shadow: 5px 5px #558abb;

/* color | offset-x | offset-y */
text-shadow: white 2px 5px;

/* offset-x | offset-y
/* Use defaults for color and blur-radius */
text-shadow: 5px 10px;

/* Global values */
text-shadow: inherit;
text-shadow: initial;
text-shadow: unset;
```
* `box-shadow: 0 0 1rem #ffc600;` Adds shadow effects around an element's frame. You can set multiple effects separated by commas. A box shadow is described by X and Y offsets relative to the element, blur and spread radii, and color. Syntax:
```css
/* Keyword values */
box-shadow: none;

/* offset-x | offset-y | color */
box-shadow: 60px -16px teal;

/* offset-x | offset-y | blur-radius | color */
box-shadow: 10px 5px 5px black;

/* offset-x | offset-y | blur-radius | spread-radius | color */
box-shadow: 2px 2px 2px 1px rgba(0, 0, 0, 0.2);

/* inset | offset-x | offset-y | color */
box-shadow: inset 5em 1em gold;

/* Any number of shadows, separated by commas */
box-shadow: 3px 3px red, -1em 0 0.4em olive;

/* Global keywords */
box-shadow: inherit;
box-shadow: initial;
box-shadow: unset;
```