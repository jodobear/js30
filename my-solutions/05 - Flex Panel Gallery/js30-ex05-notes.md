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

* `display: flex;` default stacking of items is vertical left to right.
* `flex: 1;`: evenly distribute space between items within the flexbox.
* `justify-content: center;` aligns content horizontally.
* `align-items: center;` aligns items vertically.
* `flex: 1 0 auto;` evenly distributed the container space between the items, similar to `flex: 1;`, but don't know the difference.
*
