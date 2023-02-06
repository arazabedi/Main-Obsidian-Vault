**Create a data attribute by adding a `data-` attribute to an element:

```html
<div id="test-div" 
	data-first-name="Kyle" 
	data-last-name="Cook" 
	data-active 
></div>`
```

**Read a data attribute:**

```js
const div = document.getElementById("test-div")

console.log(div.dataset) // returns a DOMStringMap
```

*A DomStringMap is essentialy an object containing all the custom data attributes of an element.

`div.dataset` will return the following:

```js
{
	active: "" 
	firstName: "Kyle" 
	lastName: "Cook" 
}
```

>[!info]
> 1. Properties are converted from snake case to camel case because object properties in JavaScript are primarily written as camel case
> 2. Data attributes without a value are assumed to have an empty string

**Write a data attribute**:

```js
const div = document.getElementById("test-div")

div.dataset.test = "Hi"
console.log(div.dataset.test)
// Hi
```

*The div now will look like this:*

```html
<div
  id="test-div"
  data-test="Hi"
  data-first-name="Kyle"
  data-last-name="Cook"
  data-active
></div>
```

**Updating a data attribute:**

```js
const div = document.getElementById("test-div")

div.dataset.firstName = "Sally"
console.log(div.dataset.firstName)
// Sally
```

^81ec94

*The div now will look like this:*

```html
<div
  id="test-div"
  data-first-name="Sally"
  data-last-name="Cook"
  data-active
></div>
```

**Delete a data attribute:**

```js
const div = document.getElementById("test-div")

delete div.dataset.active
console.log(div.dataset.active)
// undefined
```

*The div now will look like this:*

```html
<div
  id="test-div"
  data-first-name="Sally"
  data-last-name="Cook"
></div>
```

**Real World Example:**

We start with the following:

```html
<button>Open Modal 1</button>
<button>Open Modal 2</button>

<div id="modal-1">Modal 1</div>
<div id="modal-2">Modal 2</div>
```

We want to write JS so that the first button opens modal one, and the second opens modal 2. 

Using data attributes we can do this in a way that allows us to add further modals without needing to write new JS:

```html
<button data-modal-id="modal-1">Open Modal 1</button>
<button data-modal-id="modal-2">Open Modal 2</button>

<div id="modal-1">Modal 1</div>
<div id="modal-2">Modal 2</div>
```

*We've added the `data-modal-id` attribute to each button, and assigned the id of the respective modal that we wish to open as the value of each attribute*

We can now write JS to make the buttons open the modals:

```js
const buttons = document.querySelectorAll("[data-modal-id]")

buttons.forEach(button => {
  button.addEventListener("click", () => {
    const modalId = button.dataset.modalId
    const modal = document.getElementById(modalId)
    modal.classList.add("show")
  })
})
```

- We select all elements that contain the `data-modal-id`
- We then loop through them , adding a click event listener to each one
- Inside each event listener we use the modal id to get the modal link to that button
- Then we add the class `show` to the modal within the event listener so that it is now visible

Now to add a third button-modal combination, we just have to add a new button div with the data attribute `data-modal-id` and a new modal with the same id as the data attribute's value. No further JS is required!