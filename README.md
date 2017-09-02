__Lazy Template__

*Incredibly simple and yet powerful non-destructive javascript
templating system written by a lazy programmer so he can stay lazy and
yet be able to effortlessly build and manage UIs (or let web designers
to do that without breaking his UI-building javascript code).*

<!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc-generate-toc again -->
**Table of Contents**

- [Features](#features)
- [Audience](#audience)
- [Quick Introduction](#quick-introduction)
- [Interactive Examples](#interactive-examples)
- [Syntax](#syntax)
    - [Javacript](#javacript)
    - [HTML](#html)
- [More](#more)

<!-- markdown-toc end -->


# Features
- [x] Supports nested templates with for-each-like behavior
- [x] You can apply template multiple times to the same element to update UI with changed values
- [x] Support for conditional classes
- [x] Support for conditional attributes (e.g. check check-box if value is true)
- [x] Support for conditional hidding/showing/removal of elements
- [x] Super lightweight with only 2.5kB of compressed Javascript
- [x] And more...

# Audience

It is meant for programmers dynamically building UIs on front end
using AJAX and static HTML templates.  You can keep your code clean
while allowing designers to edit HTML templates without breaking your
Javascript. Allows easy maintenance and updates of UIs.

# Quick Introduction

First you need to include Lazy Templates on your page. Replace `…`
with the real path pointing to your files.

```HTML
<!doctype html>
<html>
    <head>
	    ...
        <script src="https://code.jquery.com/jquery-3.2.1.min.js"></script>
		
        <link rel="stylesheet" type="text/css" href="…/template.css"/>
        <script src="…/jquery.template.min.js"></script>
		...
    </head>
    <body>
        ...
    </body>
</html>
```

Now you can use add the `z-var="PROPERTY TARGET"` attribute to the element. The
`TARGET` is usually `.` (dot) for inserting variable as element's text
content or `@ATTR_NAME` to insert variable into named attribute on the
element. You can specify more actions separated by comma.

```HTML
<div z-var="name ."></div>
<div z-var="name .">My name is ${name}</div>

<script>
    $('div').template({'name': 'John Smith'});
</script>
```

Why to have `z-var` attribute? Because unlike other solutions using
`z-var` attribute allows you to update the same HTML with new values
because Lazy Template is non-destructive.

Note: You may use also `${PROP_NAME}` placeholder syntax on top of
`z-var`. Lazy Templates will remember the template text so subsequent
UI updates will work.

# Interactive Examples

The best example is the one you can play with.

- [Basic example](https://codepen.io/webdevelopers/pen/PpVGde?editors=1010#0) - insert variable into attribute and text
- [Simple list example](https://codepen.io/webdevelopers/pen/PpVZOQ?editors=1010#0) - iterate through the array and generate list
- [Mixed list example](https://codepen.io/webdevelopers/pen/jBdMXR?editors=1010#0) - simple variables with nested array to generate list
- [Form example](https://codepen.io/webdevelopers/pen/XMOjGm?editors=1010#0) - toggle check-boxes, set values on select-boxes, change button labels and classes...
- [UI Updates](https://codepen.io/webdevelopers/pen/jBdyVm?editors=1010#0) - example of continuous live UI updates

# Syntax

## Javacript
```javascript
 $(SELECTOR).template(DATASET [, IN_PLACE]);
```

- __`DATASET`__ - Object or Array of Objects or Object with nested Arrays or Objects with properties and their values to be used when resolving `z-var` attributes.
- __`IN_PLACE`__ - boolean value. `true`: don't clone the element prior to replacing variables, `false`: clone the element, `undefined`: clone if element has the attribute `template` otherwise replace vars in-place without cloning.

## HTML

```html
    <element z-var="[!]DATASET_PROPERTY (TARGET|ACTION)[, ...]">...</element>
```

- __`DATASET_PROPERTY`__ - property name on the object passed to `$(selector).template(DATASET)` method.
- __`TARGET`__ - use following notation
    * `.` - to insert value as TextNode,
    * `@ATTR_NAME` to insert value into attribute (Note: If `DATASET_PROPERTY` si `true` then attribute's value will be same as attribute's name, e.g. `checked="checked"` for `z-var="isChecked @checked"` if `isChecked = true`.)
    * `+` - load serialized HTML text in variable as child HTML,
    * `=` - set variable's value as form field's value.
- __`ACTION`__ - following actions are available
    * `?` - hide element if value is false,
    * `!` - remove element if value is false (note: this is destructive action - you cannot re-apply new dataset again with the same effect),
    * `.CLASS_NAME` - add/remove class if value is true/false.
- __`!`__ - "not" - negates the `DATASET_PROPERTY` value for the purpose of evaluation what `ACTION` should be taken.

To determine if `ACTION` should be taken `DATASET_PROPERTY` is converted into boolean `true` or `false`. Following values are considered `false`:
* an empty `Array` or
* an empty `Object` or
* number `0` or
* string representing numeric value zero (e.g. `"0.00"`) or
* boolean `false` or
* `null` or
* empty string

If you try to insert the plain Array object as text or value then its length gets inserted instead. You can use it to insert item counts.

```html
    <element template="(NAME|[PROPERTY])">...</element>
```

- __`NAME`__ - any name of your choice. All elements having attribute `template` are hidden by default. Applying template to such element will clone it, remove the `template` attribute and then apply the dataset. See [Simple list example](https://codepen.io/webdevelopers/pen/PpVZOQ?editors=1010#0).
- __`PROPERTY`__ - name of the property on `DATASET` object that holds nested Array or Object to be applied to this template. See [Mixed list example](https://codepen.io/webdevelopers/pen/jBdMXR?editors=1010#0).


# More

For more advanced examples and explanations see file
<code>tutorial/index.html</code> or just drop me a message what part
is unclear and I will update this documentation...
