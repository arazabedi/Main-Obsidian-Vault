___
1. **Add element to page:**

```js
const body = document.body
body.append() //can append string and also multiple things at once
//or
body.appendChild() //can't append string (requires node such as div, span, a etc.)
```

___

2. **Create element then add to page:**

```js
const div = document.createElement()
body.append(div)
```

___

3. **Add text to div:**

```js
div.innerText = "Hello World"
//or
div.textContent = "Hello World 2`"
```

>[!info]
>innerText simply copies the text in your html while textContent copies exactly as your html presents using css

___
4. **Add HTML:**

```js
div.innerHTML()
//or create new element and append manually (this is more secure)
const strong = document.createElement('strong')
strong.innerText = "Hello World 2"
div.append(strong)
```

___

5. **Remove element:**

```js
const spanHi = document.querySelector("div")
const spanBye = document.querySelector("#bye")

spanbye.remove() // removes from dom but remains within js for future use

//or

spanbye.removeChild(SpanHi)
```

___

6. **Get attribute**

```js
spanHi.getAttribute("id")
//or
spanHi.id
```

___

7. **Set attribute:**

```js
SpanHi.setAttribute("title", "Batman Begins")

//or

spanHi.id = "Batman Begins"
```

___
8. **Remove attribute:**

```js
spanHi.removeAttribute("id")
```

___

9. **Add [[Data Attributes]]:**

```js
spanHi.dataset // no () after dataset
```

___

10.  **Modify classes:**

```js
spanHi.classList // return all classes
spanHi.classList.add("new-class") //add
spanHi.classList.remove("hi1") // remove
.toggle("hi3") // add or remove based on whether it's already present or not
.toggle("hi3", true) // add
.toggle("hi3", false) //remove

```

___

11. **Modify element style:

```js
spanHi.style.color = "red"
spanHi.style.backgroundColor = "red"
```

>[!warning]
>This method only works with inline styling. If stylesheets are involved, use the below `getComputedStyle` function.

___

12. **Modify and read computed styles:**

```js
const paragraphStyle = getComputedStyle(document.getElementById("para"));

console.log(paragraphStyle.fontStyle); // italic
```

___

>[!tip]
>Always create and set up element properties first before inserting them into the DOM so that you preserve performace!

