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

<img src="./img/flask_architecture.png" alt="oops! image is not found" title="flask architecture" width="550"/>

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

Example 1 (from code example Forms Basics)

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

<img src="./img/form_basics.png" alt="oops! image is not found" title="form basics" width="400"/>

The `email` input type will let the browser check if it's a valid email with `@`. `value` is prefilled.

The `password` type hides the input in the box. `value` is what's prefilled and hidden.

The `submit` type is a button where `value` has the text shown on the button.

The `color` type is interesting but not commonly used, it lets you select from a color palette.

`GET` will send back the info to our **action** URL.

`POST` submits data to be processed.

Forms must set label for each text box in order to let the user see which field is which **in the UI**. The `for` in `<label>` must match the `id` in `<input>` to label the input properly.

Example: (from example Form Labels)

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

<img src="./img/form_labels.png" alt="oops! image is not found" title="form labels" width="600"/>

`action` is the action that gets triggered upon form submission. Making it an URL is a redirect.

`value` in the text input type tag is a pre-populated string that is shown in the text input box before typing. It is also the value that actually gets submitted for the field.

`placeholder` is a hint to the user when the field is empty and it is greyed out.

For type "password", `value` is prefilled and hidden, `placeholder` is a hint without actual filled value and is not hidden.

#### Form Selections

When two input `radio` buttons share the same `name`, only one can be selected.

Example: (from example form seletions)

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

<img src="./img/form_selections.png" alt="oops! image is not found" title="form selections" width="600"/>

`<select>` gives a dropdown selection of `<option>`s. Each option has a `value`. The `value` of the option selected will be assigned to `name` (variable name) of the `<select>` and the backend can see `name = value` for this dropdown.

`<textarea>` is a big text box which can be set with # rows and columns.

Note that `<submit>`'s value is just the string shown on the submit button.

Once hit submit, the URL will be updated and a part in the format `?name=value&name=value&name=value` will be appended.

## CSS Crash Course

CSS = Cascading Style Sheet

CSS controls the color, background, borders and much more.

1. Create a `.css` file
2. Use CSS syntax to link element tags
3. Add style name-value pairs
4. Connect CSS to HTML

### 1. Colors

Example: (from Part1_master/css)

```
/*Colors can be names, or codes*/

h1{
  color: blue;
}

li {
  color: rgb(0,200,0);
}

/*Search Google for hex color, it has a hex color picker*/
p{
  color: #000000;
}

/*a is alpha, controls transparency, 1 is fully opaque, 0 fully transparent*/
h4{
  color: rgba(13,90,140,0.5)
}
```

The general format is shown below, don't forget the `;`

```
Selected Tag {
  property: value;
}
```

To link the CSS file to HTML, add the following in the `<head>` section to the HTML.

```
<link rel="stylesheet" href="Part1_master.css">
```

`rel` is the relationship attribute of the link, it says the CSS is a `stylesheet` of the HTML.

`href` points to the path of the CSS file.

The final result is shown below.

<img src="./img/css1.png" alt="oops! image is not found" title="CSS example 1" width="300"/>

### 2. Backgrounds and Borders

Example: (from Part2_master.css)

<img src="./img/css_html2.png" alt="oops! image is not found" title="CSS example 2 HTML" height="320" width="400"/>

<img src="./img/css_code2.png" alt="oops! image is not found" title="CSS example 2 Code" height="320" width="400"/>

`background` can be an url to an image, set `no-repeat` to avoid tiling. `background-repeat` can be `repeat-x` or `repeat-y` for x and y axis only.

For `border`, `border-style` and `border-width` are required attributes. Use one line to avoid 3

```
border: orange 10px dashed;
```

Final result:

<img src="./img/css2.png" alt="oops! image is not found" title="CSS example 2" width="600"/>

### 3. `class` and `id`: CSS Selector

**This is the most important one for CSS. We can select by `id` or `class`.**

Every HTML element can accept a `class` or `id` attribute. CSS can link to them by

- `.` for `class`
- `#` for `id`

**`class`s are for styling multiple different elements.**

**`id`s are for a single and unique element.**

Example: from CSS Part3

<img src="./img/css_html3.png" alt="oops! image is not found" title="CSS example 3 HTML" height="320" width="400"/>

<img src="./img/css_code3.png" alt="oops! image is not found" title="CSS example 3 Code" height="320" width="400"/>

Later, Bootstap will define classes for us.

**To summarize, CSS can style the HTML based on tags, classes and ids.**

### 4. Inspect Elements in Browser

In Chrome we can inspect the HTML and CSS in the developer tool. We can even edit it locally to see changes. For example, open Google and change it's styling locally.

To go back to the original site, just hit refresh.

### 5. Fonts

Not every font is available on each OS. Mac, Windows and Linux have different fonts.

Use [Google Fonts API](https://fonts.google.com/) to change fonts.

1. Add a link to Google fonts API in the HTML
2. Add the name for the `font-family` from Google Fonts API.

You can get the link and name from the Google Fonts webpage.

Example: look at CSS Part5 files

## Bootstrap 4

TBD
