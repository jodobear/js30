# JS30 Ex05: Flex Panel Gallery

We are given a web page with five `div` elements with class `panel`, each containing three `p` elements with some text. They are wrapped inside another `div` with class `panels`. Currently, these divs are stacked vertically and aren't interactive. We want to display these divs vertically and have only the middle `p` element of each `div` displayed; when a user clicks on that element, we want to expand that element and bring the two other `p` elements back into view. Update the CSS and write the JavaScript code necessary to bring this interactivity to the page.

Most of this challenge focuses on working with CSS3 flexible boxes, or flexbox.

## Guide

Flex box layouts consist of a flex container which contain flex items. We'll use the `panels` class as the flex container, and the `panel` class as the flex items. By default, flex items are only as wide as they need to be in order to display their contents, but we want them to take up the entire flex container. Allow the flex items to take up equal space and fill out the flex container by allowing them to grow. Given that we want the content of each flex item to be flexible as well, we're going to display the `panel` class as both a flex item AND a flex container; this means that elements with the `panel` class will adjust themselves with respect to their flex container (`div` with class `panels`), and the contents within those elements (in this case, the three `p` elements) will adjust themselves with respect to their own flex container (`div` element with class `panel`).

Horizontally and vertically center the content of the `panel` class, and modify the styling for any children of the panel class (`.panel > *`) so that they are displayed as flex items and take up 1/3 of their respective flex container. Create new style definitions for the first and last child elements of the `panel` class to push them off the page and to bring them back in when their parent panel is selected, and update the CSS for the panel open class so that the selected div will be 5x larger than the other div elements. Holy CSS, Batman!

Finally, we'll write some JavaScript code to attach event listeners to each panel element that will fire when an panel is clicked on and call their respective event handlers; one event handler function will adjust the size of the panel, and the other will be responsible for bringing in the p elements that we pushed off earlier.

### CSS

Update the styling applied to the panels class to display as a flex container:
```
.panels {
  /* ... */
  display: flex;
}
```
Update the styling applied to each panel so that they equally maximize their width to fill out the flex container:
```css
    .panel {
      /* ... */
      flex: 1;
    }
```
Update the styling applied to the panel class so that each panel is also a flex container and displays its content in columns:
```css
.panel {
  /* ... */
  display: flex;
  flex-direction: column;
}
```
Update the styling applied to the child elements of the panel class so they are treated as flex items and are center justified:
```css
.panel > * {
  /* ... */
  align-items: center;
  display: flex;
  flex: 1 0 auto;
  justify-content: center;
}
```
Create new style definitions for the first and last child elements of the panel class so that the content is pushed off the main page until the panel is selected by the user:
```css
.panel > *:first-child {
  transform: translateY(-100%);
}

.panel.open-active > *:first-child {
  transform: translateY(0);
}

.panel > *:last-child {
  transform: translateY(100%);
}

.panel.open-active > *:last-child {
  transform: translateY(0);
}
```
Update the styling applied to the panel open class so that the selected panel takes up 5x the space of the other flex items:
```css
.panel.open {
  /* ... */
  flex: 5;
}
```
### JS

1. Declare & define a variable as a reference to all elements with a class panels.
2. Iterate through the HTML Node elements that the variable is referencing and attach an event listener for the click event to each element, providing the name of a yet-to-be-defined function as the event handler.
3. Repeat step 2, this time attaching an event listener for the transitionend event and providing a different function name for the event handler.
4. Define the function for Step 2 to toggle the class open on the function context.
5. Define the function for Step 3 to toggle the class open-active on the function context IF the event which triggered this function has a property name that includes the word 'flex'.

## Learnings

### CSS

#### `panels`
* `display: flex;` default stacking of items is vertical left to right and they will size according to the content they have.
* `flex: 1;`: each of the panel(element) will evenly distribute space between items within the flexbox. I think of it as 1:1.

#### `panel`
* To get the text (childern of each `panel` which is child of `panels` flexbox) we first make the `panel` a flexbox too; `display: flex;` (so, now we have flexbox nested inside a flexbox).
* This makes all the children align horizontally from left to right so, we apply, `flex-direction: column;` to align them vertically.
* Now the text is aligned but, not in center so, we apply `justify-content: center;` which when `flex-direction` is `row`(defalut) will align content horizontally centered else when `column` aligns content vertically.
* `align-items: center;` aligns items vertically. Implemented by Wes but, i don't think it's required(?)

#### Flex children
* `.panel > *` syntax to select all the children of the selected node.
* `flex: 1 0 auto;` evenly distributed the container space between the items, similar to `flex: 1;`, but don't know the difference. In fact, `flex: 1;` itself works, didn't need `flex: 1 0 auto;`
* At this stage, all the text was aligned to the top margin. To center the text we converted the children into flexboxes too; `dispaly: flex;`
* Then all the text was aligned to top-left corner so we did the following:
```css
display: flex;
justify-content: center;
align-items: center;
```
which aligned the text to absolute center of eact of the elements and was the same as doing this:
```css
display: flex;
flex-direction: column;
justify-content: center;
```
Here, before applying `align-items: center;` items were centered horizontally at the top margin and this centered them vertically.

#### Getting the Text Off Screen

* `.panel > *:first-child { transform: translateY(+/-100%); }` used this.
  * `:first-child` pseudo selector for first of the children of the parent slector.
  * Using the `transform` property, we apply `translateY(%)` which if `+/-50%` depending on which margin it is at, will move the element 50% off screen, while `100%` will move it completely off screen, `+100%` for bottom & `-100%` to take it off the top.
.panel.open-active > *:first-child { transform: translateY(0%); }
  * Then we applied a another class `.open-active` to the above line and passed `0%` to both first & last child so that they are back in view, like so; `.panel.open-active > *:first-child { transform: translateY(0%); }`. **NOTE** does it matter `.panel.open-active` no space or `.panel .open-active`(?)

#### Expanding the `panel`

We simple added `flex: 5;` to the `.open` class chained to `.panel` which says, when applied use 5 times the space.

### JS
 1. We first got all the individual panels, `.panel` (not `.panels`).
 2. Created a function to toggle the `open` class. One thing of note, arrow gave the error `Uncaught TypeError: Cannot read property 'toggle' of undefined at HTMLDivElement.toggleOpen`, normal function worked. Check why or ask on stackoverflow.
3. After that we want to bring in the text AFTER the transition of expansion of the panel is finished, so, as we did before we listen for `transitionend` event pass it to a new function, check for the event's `propertyName` and toggle our `open-active` class. **NOTE** here for cross browser support, the check was `e.propertyName.includes('flex')` since different browsers name the property differently.

