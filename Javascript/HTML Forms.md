1. Use the ***form*** attributes `action` and `method` to determine *where* and using *what* HTTP method the form data should be sent to.

2. Use the ***input*** attribute `name` to determine the name of the form control (input box). 

3. Use the ***label*** attribute `for` to link the label to an ***input*** field with an `id` of the same name (allows you to click the label to highlight the input field).

4. Use the ***input*** attribute `type` to specifiy the input type. The type `password` hides what you're typing

5. Use the ***button*** attribute `submit` to allow the use of the ENTER button to submit the form. This allows you to have other buttons that don't submit. The `reset` button type for example removes all inputs to the form.

6. Use the ***input*** property `value` to set the default values for the input boxes, and `placeholder` to set the placeholder

7. Use the ***input*** property `required` to throw an error if the input field isn't filled out. This attribute has not value.

```html
<form action="results.html" method="GET">
	<div>
		<label for="name">Name</label>
		<input type="text" name="name" id="name" placeholder="Lex" required>
	</div>
	<div>
		<label>Password</label>
		<input type="password" name="password" required>
	</div>
	<div>
		<button type="reset">Reset</button>
		<button type="submit">Submit</button>
	</div>
</form>
```

>[!info]
>- GET will append the URL
>- POST is useful for saving some information to a server. 
>- Browsers can only render GET requests!


You can also nest the input inside the label to link the two together : 

```html
	<div>
		<label>
			Name
			<input name="name">
		</label>
		
	</div>
```

___
**Input Types**

***type="..."***

`email` - throws an error if the email is invalid (not the best validation) and gives smartphone users the email-specific keyboard.

`number` - brings up arrow keys and does not allow non-numbers. Add a `min` and a `max` attribute for extra customisability.  Add `step` to modify the step on using the up and down arrow buttons

`date` - takes `min`/`max` 

`hidden` - user can't interact with this but on submission will send the name/value to the server/page

`file` - see below

`tel`

`url`

`color`

`checkbox` - multiple option square checkbox

`radio` - single option circle checkbox. The `name` attribute has to be the same on each checkbox.

```html
    <div>
      <input type="radio" id="huey" name="drone" value="huey"
             checked>
      <label for="huey">Huey</label>
    </div>

    <div>
      <input type="radio" id="dewey" name="drone" value="dewey">
      <label for="dewey">Dewey</label>
    </div>

    <div>
      <input type="radio" id="louie" name="drone" value="louie">
      <label for="louie">Louie</label>
    </div>
```

___
**Dropdown Menu**

```html
<select name="pets" id="pet-select" multiple>
    <option value="">--Please choose an option--</option>
    <option value="dog">Dog</option>
    <option value="cat">Cat</option>
    <option value="hamster">Hamster</option>
    <option value="parrot">Parrot</option>
    <option value="spider">Spider</option>
    <option value="goldfish">Goldfish</option>
</select>
```
<select name="pets" id="pet-select" multiple>
    <option value="">--Please choose an option--</option>
    <option value="dog">Dog</option>
    <option value="cat">Cat</option>
    <option value="hamster">Hamster</option>
    <option value="parrot">Parrot</option>
    <option value="spider">Spider</option>
    <option value="goldfish">Goldfish</option>
</select>

Use the  attribute  `multiple` to allow for the selection of multiple options.

___

**Text Area**

```html
<textarea id="story" name="story"
          rows="5" cols="33">
It was a dark and stormy night...
</textarea>

```

<textarea id="story" name="story"
          rows="5" cols="33">
It was a dark and stormy night...
</textarea>

Use the `<textarea></textarea>` element for multi-line input.

___

File Uploads

1. Use the type `file`.
2. Add `enctype="multipart/form-data"` to the `form` tag:

```html
<form action="results.html" method="GET" enctype="multipart/form-data>
	...
</form>
```
