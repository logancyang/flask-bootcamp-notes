# Flask Full Stack Part 1: Frontend Quick Walkthrough

(course by Jose Portilla on Udemy)


## Overview

Flask is a super simple Python web framework and is also scalable with a lot of 3rd party libraries. Choosing Flask rather than
NodeJS because Python has nice ecosystem in machine learning. What Flask does in general:

- Connect with UI
- Connect to database and handle CRUD (create, read, update, delete)

For example, it can handle HTML forms via library `WTForms`. SQLite is sufficient as a database for a small website.

Flask is a middle man between the frontend UI and the database backend.

We will use Jinja templates to grab information from Python and Flask to send as HTML.

[pic_flask_and_frontend_jinja]

## HTML quick reference

`<!DOCTYPE html>` tells it's an html file. `<head>` contains metadata, title on the tab and links to javascript, `<body>` contains content such as forms, styles, headers, etc.

### Basic tags, list, div, span, attribute

`<h1>`: heading one

`<h6>`: heading six

`<p>`: paragraph

`<strong>`: bold

`<em>`: italics

`<br>`: line break

For full reference, go to [Mozilla HTML elements reference](https://developer.mozilla.org/en-US/docs/Web/HTML/Element).

[CodePen](https://codepen.io/) and [JSFiddle](https://jsfiddle.net/) are good online test grounds.

`<ol>`: ordered list

`<li>`: list item

`<ul>`: unordered list

Lists can be nested.

`<div>` stands for division.

`<div>` and `<span>` can separate the HTML page into sections.

`<div>` is for larger division/block of elements, `<span>` is for substring such as

```
<p>Here is <span class='myclass'>some text</span>. woohoo! </p>
```

for doing styling on `myclass`.

#### HTML Attributes

`<img src="<link to image>" alt="Uh oh! No image">`

`<link to image>` can be an url online or a path to local file. Next sections will show how to organize static files in Flask.

`<a href="<some url>">My link here</a>`

Again, `<some url>` can be an URL online or a path to another html file locally.

Note that `<img>` is a self closing tag but `<a>` is not.

### HTML Forms

Consist of `<form>` and `<input>` tags.

Example 1

```
<form>

    <h1>Log In</h1>
    <h2>Please Input your Password and Email</h2>
    <input type="email" name="useremail" value="Email Here">
    <input type="password" name="password" value="Password">
    <input type="submit" name="" value="Enter">

    <h1>Choose a Color!</h1>
    <h2>Click on the Button when ready</h2>
    <input type="color" >
    <h2>Enter some Text!</h2>
    <input type="text" name="" value="Text goes here">

</form>
```

The `email` input type will let the browser check if it's a valid email with `@`.

The `password` type hides the input in the box.

The `submit` type is a button where `value` has the text shown on the button.

The `color` type is interesting but not commonly used, it lets you select from a color palette.

`GET` will send back the info to our **action** URL.

`POST` submits data to be processed.

Forms must set label for each text box in order to let the user see which field is which in the UI. The `for` in `<label>` must match the `id` in `<input>` to label the input properly.

Example:

```
<!-- Upon submitting the form will perform the action (a redirect) -->
    <form action="http://www.google.com" method="get">

      <label for="email">EMAIL:</label>
      <input type="email" id="email" name="useremail" value="Email Here">


      <label for="pass">PASSWORD:</label>
      <input type="password" id="pass" name="password" placeholder="Password">

      <!-- Validation -->
      <!--  Usually do a lot more validation with Backend-->
      <!--  Use the attribute: required to require input-->

      <label for="userinput">TEXT:</label>
      <input type="text" id="userinput" name="input" placeholder="Enter Text Here" required>

      <input type="submit" >

    </form>
```

`action` is the action that gets triggered upon form submission. Making it an URL is a redirect.

`value` in the text input type tag is a pre-populated string that is shown in the text input box before typing. It is also the value that actually gets submitted for the field. `placeholder` is a hint to the user when the field is empty and it is greyed out.

#### Form Selections

When two input `radio` buttons share the same `name`, only one can be selected.

Example:

```
        <form method="get">
          <h3>Do you already own a dog?</h3>
          <label for="yes">Yes</label>
          <input type="radio" id="yes" name="dog_choice" value="yes">

          <label for="no">No:</label>
          <input type="radio" id="no" name= "dog_choice" value="no">

          <p>How clean is your house (Rated 1-3 with 3 being cleanest))</p>
          <select name="stars">
            <option value="Great">3</option>
            <option value="Okay">2</option>
            <option value="Bad">1</option>
          </select>

          <p>Any other comments?</p>

          <textarea name="name" rows="8" cols="80"></textarea>

          <input type="submit" name="" value="Submit Feedback">

        </form>
```

`<select>` gives a dropdown selection of `<option>`s. Each option has a `value`. The `value` of the option selected will be assigned to `name` (variable name) of the `<select>` and the backend can see `name = value` for this dropdown.

`<textarea>` is a big text box which can be set with # rows and columns.

Note that `<submit>`'s value is just the string shown on the submit button.

Once hit submit, the URL will be updated and a part in the format `?name=value&name=value&name=value` will be appended.

## CSS

CSS = Cascading Style Sheet



## Bootstrap 4

TBD
