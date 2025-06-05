Involves the full range of uses of Power Platform. The exam expects you to be able to design, develop, test, secure, and troubleshoot Power Platform solutions.

Expects knowledge of what PP is and how it can be used.

- Create a technical design
- Configure Dataverse
- Create and configure PowerApps (not included in this course - check PL 900, 200, 100)
- Configure business process automation (not included in this course)
- Extend the user experience
- Extend the platform

Majority of the course covers (see syllabus):
- Extend the user platform
- Extend the platform
- Develop integrations

# Apply Business Logic Using Client Scripting (Run Functions Using Event Handlers)

## Creating a Power Platform Environment and a Model-Driven App (JS/TS) With Sample Data

make.powerapps.com -> gear icon (top right) -> admin center -> environments -> new

Choose type (e.g. Production) and add Dataverse. On the next page select security groups -> open (anyone has access).

Go to Power Apps -> tables -> pick a table (e.g. accounts) -> create an app

If no data in table:
Home -> settings -> admin center -> environments -> click on the specific environment -> settings -> data management -> sample data

## What Event Handlers Can I Use? Adding a Hello World Notification to a Form

Some functionality requires JS/TS - for example changing fields depending on whether the account has a website etc...

To change a form, go into tables, find the form, select it, the click edit.

Example of some Power Platform JS:

```js
this.formOnLoad = (executionContext) => {
  execution
    .getFormContext()
    .ui.setFormNotification("Hello world", "INFO", "IDUnique220912");
};
```

Create a library on the left with Form libraries -> add library => + new web resource -> select type (to narrow down) -> choose file -> save and publish.

You then need to go to Form Libraries (button) on the top toolbar and add your new library.

## Setting and Getting the Value of Fields

You get check details (e.g. logical name of a field) in the Form by going to the edit page, selecting the field, and checking the properties (edit table column => advanced options). You can also search for the field on the left.

Let's use JS to set a value:

```js
this.formOnLoad = (executionContext) => {
  const formContext = executionContext.getFormContext();
  formContext.ui.setFormNotification(
    "Hello world v2",
    "INFO",
    "IDUnique220912"
  );
  formContext.getAttribute("fax").setValue("123-4567");
};
```

There's no need to save and publish the form - the library and the form are independent.

But you can also check to see whether there is already an existing a number before setting the new value:

```js
this.formOnLoad = (executionContext) => {
  const formContext = executionContext.getFormContext();
  formContext.ui.setFormNotification(
    "Hello world v3",
    "INFO",
    "IDUnique220912"
  );
  if (formContext.getAttribute("fax").getValue() == null) {
    formContext.getAttribute("fax").setValue("123-4567");
  }
};
```

This will only set the value if the existing value is null!

Btw, when using `getAttribute`, you must use the logical name (that can can check in the properties of field - specific details at the top of this section).

## executionContext.getFormContext() Object

There are other contexts (e.g. getGridContext and Xrm), but generally you use form context.

Methods on getFormContext:
- data (contains attribute - same as below, entity (current record but only those shows on the current form) - contains attributes -> controls & save method, process -> stages, steps)
- ui
- getAttribute (allows you to get and then set using setAttribute)
- getControl

Extras:
- addOnLoad/removeOnLoad
- getIsDirty (modified but not saved)
- isValid (any required columns that aren't filled in?)
- refresh
- save

![[Pasted image 20250108182417.png]]

getFormContext.ui methods:
- formSelector.items - query which forms are currently available for the current user based on security roles
- navigation.items - options in the navigation bar
- controls - controls on the form
- process - business process rules stages and steps on a form
- quickForms
- tabs -> tabs.sections
- addOnLoad
- close
- getFormType - 0 undefined, 1 create, 2 update, 3 read only, 4 disabled, 6 bulk edit
- get viewPortHeight and getViewPortWidth
- setFormEntityName (form title)
- setFormNotification(message, level, uniqueID)
- clearFormNotification(uniqueID)

getFormContext.getControl methods:
- addNotification/clearNotification
- getAttribute
- getControlType (standard, lookup, choices)
- getDisabled, setDisabled
- getName
- getOutput
- getParent
- getVisible/setVisible
- setFocus
- more?

## Adding Field Notification

The following code will set a default value of 123-4567 to all form fax fields in the form you add the JS to (if there is no existing value in the field):

```js
this.formOnLoad = (executionContext) => {
  const formContext = executionContext.getFormContext();
  formContext.ui.setFormNotification(
    "Hello world v4",
    "INFO",
    "IDUnique220912"
  );
  if (formContext.getAttribute("fax").getValue() == null) {
    formContext.getAttribute("fax").setValue("123-4567");
    formContext.getControl.addNotification({
      messages: ["Fax number set to default."],
			notificationLevel: "Recommendation",
			uniqueID: "IDUnique220912-2",
			// actions:
    });
    formControl.getControl("fax).clearNotification();
  }
};
```

## Debugging errors

You can add a function to hide a particular line on a form by adding it to an event handler (on the right of a form edit page) - remember to save & publish as you are changing the form itself, not just the library:

```js
this.AddressStreet3Hide = (executionContext) => {
  const formContext = executionContext.getFormContext();
  formContext.getControl("address1_line3").setVisible(false);
};
```

In the video, this does not actually hide the line because there is an error (shown on the form itself - in you model-driven app). It says: `Cannot read properties of null (reading: 'setVisible'`. This means that the argument for the `getControl` method is wrong as all form fields should have that method.

To debug, go to dev tools then `CMD + P` (`CNTL + P` on Windows) to open search and then find the JS file. Put a breakpoint on the line you think is going wrong.

Next go to the form and do an action that tiggers your event handler. On the right-hand side of dev tools there should be a section called 'Watch', and under that your functions and what they return.

In the code section of dev tools, you can play around with the code in order to figure out how it works.

## Getting a Composite Field (Field With Multiline Text Such as the Address Field)

Use `composite_compositionLinkControl` to access individual fields in multi-field/composite fields. 

```js
this.AddressStreet3Hide = (executionContext) => {
  const formContext = executionContext.getFormContext();
  formContext.getControl("address1_composite_compositionLinkControl_address1_line3").setVisible(false);
};
```


How to hide address line 3 when address line 2 is empty:

```js
this.AddressStreet3Hide = (executionContext) => {
  const formContext = executionContext.getFormContext();
  if (formContext.getAttribute("Address1_line2").getValue() == null){ formContext.getControl("address1_composite_compositionLinkControl_address1_line3").setVisible(false);
  } else { formContext.getControl("address1_composite_compositionLinkControl_address1_line3").setVisible(true);
  }
};
```
# Create a Command Button Function

You can add buttons to the top of a form page (where the save, add, delete buttons are) by going to the app you want to modify, then click edit, then pages.

On the left hand side there will be a section with the same title as the form in your app (e.g. Account). Under that there will be Account form and Account View. If you just hover over Account and click the three dots (...) you can click edit command bar.

Now choose which command bar to modify:
- Main grid - View that shows all the accounts in a table
- Main form - View that shows a specific row of the accounts table (each account)
- Sub-grid view - for use with sub-grids
- Associated view - When a grid of related records appear in the navigation page

# Creating Our First Plugin

- You can use plugins to run .NET code on Power Platform
- 