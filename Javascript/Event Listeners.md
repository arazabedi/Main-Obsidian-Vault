**Setting Up a Basic Event Listener**

```js
const grandparent = document.querySelector(".grandparent")
const parent = document.querySelector(".parent")
const child = document.querySelector(".child")

grandparent.addEventListener('click', e => {
	console.log(e);
})
```

The `addEventListener()` function can take two or three parameters:
1). Type of event (e.g. 'click')
2). Callback (function that runs on event occurence)
3). Option (object with parameters)

>[!info]
>If two event listeners are 'listening' for the same event, then their callbacks will be executed in the order the code was written

**Event Bubbling and Capturing**

Event listeners include anything under the umbrella of what they're listening for. This means that if you click on the child or grandchild of an element, you are also clicking on the grandparent element as well.

If you have a click event listener for each of the child, parent and grandparents, with a `console.log` callback function on each one, then the browser will (on clicking the child) execute each log starting with the child and working its way up to the grandparent.

This process of going up the DOM tree is called ***Event Bubbling***.

The third parameter of an event listener is where ***Capturing*** comes into play:

```js
grandparent.addEventListener('click', e => {
	console.log(e);
}, { capture: true })
```

*By default, capture: false unless specified (as above)

By adding the object `{ capture: true }` as a third argument we tell the browser to execute the callback prior to the event bubble floating upwards. 

This event is now a 'capture' event. A capture event has no bubble phase.

Capture occurs prior to bubbling.

**Stopping Event Propogation**

```js
parent.addEventListener('click', e => 
	e.stopPropagation()
	console.log(e);
}, { capture: true })
```

`stopPropagation()` stops the propagation of an event (both the capturing and bubbling). You can use it in either phase.

**Run Event Once Only**

Pass `{ once: true }` to the third parameter

```js
parent.addEventListener('click', e => 
	console.log(e);
}, { once: true })
```

**Run Event Multiple Limited Times**

`.removeEventListener()`

```js
parent.addEventListener("click", printHi)

setTimeout(() => {
	parent.removeEventListener("click", printHi)
}, 2000)
```

**Event Delegation**

A div added using JS after an event listener will not be involved in the listening process.

You could add a whole new event listener, but this is clunky.

Instead we put an event listener on the whole document, then in our callback we use the `.matches()` function.

```js
document.addEventListener("click", e => {
	if (e.target.matches("div")) {
		console.log("hi")
	}
})
```

Essentially, we are telling the browser:
`Log "hi" if ...`
`The element that triggered the event...`
`Matches the CSS selector given ("div" in this case)`

`e` is an event object. It has a `target` property with a value of the element object that triggered the event.

An element object has q method `matches()` that returns true if an element matches a specific CSS selector.

You can make this into a function:

```js
function addGlobalEventListener(type, selector, callback) {
	document.addEventListener(type, e => {
		if (e.target.matches(selector))
		callback(e)
	})
}
```

> [!tip] 
> Use the function above in most cases. Just chuck the function code into your .js file and use it in the place of manually typing addEventListener code out.
