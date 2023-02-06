
Svelte applications are built using components (.svelte files).

A .svelte file can contain HTML, CSS, and JavaScript code.

You must mark these with their respective applicable tags:

| Language | Tag                         |
| -------- | --------------------------- |
| HTML     | `<h1>`, `<div>`, `<p>` etc. |
| CSS      |             `<style>`                |
| JavaScript         |       `<script>`                      |

You can declare variables using JavaScript code, then use those variables inside HTML code with curly braces:

```html
<h1> Hello {name}! </h1>

<script>
	let name = 'world'
</script>

// Hello world!
```

You can also add JavaScript code inside curly braces:

```html
<h1>Hello {name.toUpperCase()}!</h1>

<script>
	let name = 'world'
</script>

// Hello WORLD!
```

We can also use curly braces within attributes. When an attribute has the same name as a variable, you can simply put the attribute name in curly braces:

```html
<script>
	let src = '/tutorial/image.gif';
	let name = 'Rick Astley';
</script>

<img {src} alt="{name} dances.">

// Displays an image of Rick Astley dancing
```

Style tags within a .svelte file only affect the elements within that file.

You can import components (e.g. Nested.svelte into App.svelte) using:

```html
//App.svelte
<script>
	import Nested from './Nested.svelte';
</script>

<Nested/>

// Displays whatever is inside Nested.svelte
```

*Note that components are capitalised to differentiate them from HTML tags

If your variable contains HTML (i.e. if the variable is a string with HTML tags inside it), then you need to add `@html` before the variable name inside your curly braces.

```html
<script>
	let string = `This string contains some <strong>HTML!!!</strong>`;
</script>

<p>{@html string}</p>
```

Create a SvelteKit app using:
```bash
npm create svelte@latest myapp
```

Use the `on:` directive to listen to DOM events:

```html
<script>
	let count = 0;

	function incrementCount() {
		count += 1;
	}
</script>

<button on:click={incrementCount}>
	Clicked {count} {count === 1 ? 'time' : 'times'}
</button>
```

If you want a variable to update whenever its referenced values change, use `$:`:
``
```js
	let count = 0;
	$: doubled = count * 2;
```

You can also run arbitrary statements reactively:

```js
$: console.log('the count is ' + count);
```

*This will log the string every time the `count` variable's value changes*

You can group statements together, and even put the  `$:` in front of things like `if` blocks:

```js
$: if (count >= 10) {
	alert('count is dangerously high!');
	count = 9;
}
```

`$:` will only re-run code if a variable is re-assigned. For example when `count` is updated on every click. It will ***not** re-run if an `array` or `object` is mutated (as mutation is not assignment).

`.push` is an example of a function that will mutate an `array`. Also included are the array methods `.pop`, `.shift`, `.splice`, as well as object methods `Map.set` and `Set.add`.

You can assign a variable to itself to avoid this pitfall:

```js
let numbers = [1, 2, 3, 4];

function addNumber() {
	numbers.push(numbers.length + 1);
	numbers = numbers;
}
```

Or 

```js
let numbers = [1, 2, 3, 4];

function addNumber() {
	numbers = [...numbers, numbers.length + 1];
}
```

Assignments to the *properties* of arrays and objects — e.g. `obj.foo += 1` or `array[i] = x` — work the same way as assignments to the values themselves.

But, indirect references won't trigger reactivity.

>[!tip]
>A simple rule of thumb: the updated variable must directly appear on the left hand side of the assignment.

Use `export` to pass data (properties/props) from a component to its children:

```html
//Nested.svelte
<script>
	export let answer;
</script>
```

You can specify a default value for props.

Components are almost like functions in the sense that you can store some code within a component and then re-use it. It also permits the use of 'parameters' by adding JS inside curly braces.

```html
//Nested.svelte
<script>
	export let answer = 'a mystery';
</script>

<p>The answer is {answer}</p>
```

```html
//App.svelte
<script>
	import Nested from './Nested.svelte';
</script>

<Nested answer={42}/>
<Nested/>

// The answer is 42
// The answer is a mystery
```
