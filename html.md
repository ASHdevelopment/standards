# HTML

## Table of Contents
 1. **[Attribute Ordering](#attribute-ordering-within-tags)** [ [Attribute Order](#11-attribute-order) ]
 1. **[Quotes](#quotes)**
 1. **[Inputs](#inputs)**
 1. **[Buttons](#buttons)** [ [Submit Buttons](#31-submit-buttons) ] [ [Links](#32-links) ] [ [Buttons](#33-buttons) ]
 1. **[Forms](#forms)**
 1. **[Comments](#comments)** [ [When Things Get Nesty](#51-when-things-get-nesty) ]
 1. **[Semantics](#semantics)** [ [`<strong>` vs `<b>`](#61-strong-vs-b) ] [ [`<em>` vs `<i>`](#62-em-vs-i) ]
 1. **[Do NOT Use](#do-not-use)**


## Attribute Ordering
 > Makes code easier to read and easier for a new developer to jump in and find what they are looking for.

### 1.1 Attribute Order
Ordering from the beginning of the tag to the close of the tag is as follows:

1. id/for
1. className
1. tag specific (eg: href, type, action)
1. element state (eg: value, checked, readonly)
1. ARIA/accessibility
1. data- attributes


```html
<!-- Bad -->
<input type='...' class='...' id='...' .... etc. />
```

```html
<!-- Good -->
<input id='...' class='...' type='...' ... etc. />
```

## Quotes
> Hint - it's easier to type single quotes.
- Use whatever is comfortable to you
- Do NOT go back to change code to suit your style

```html
<!-- Bad -->
<div class="class1"> <!-- do not change -->
    <p class='class2'>This is paragraph content.</p>
</div>
```

```html
<!-- Good -->
<div class='class1'>
    <p class='class2'>This is paragraph content.</p>
</div>

<div class="class1">
    <p class="class2">This is paragraph content.</p>
</div>
```

## Inputs
All inputs and labels must have an explicit relationship.
> Why? Having an explicit relationship is recommended by [www.w3.org](https://www.w3.org/WAI/tutorials/forms/labels/) to ensure that assistive technology is able to reference the correct label when presenting a form control.

```html
<!-- Bad: implicit -->
<label>
    First name:
    <input type="text" name="firstname">
</label>
```

```html
<!-- Good: explicit -->
<label for="firstname">First name:</label>
<input id="firstname" type="text" name="firstname">
```

## Buttons
> Buttons can submit a form, be a link styled like a button, or simply just a button.

Buttons should always have the `type` attribute set to `button` unless it is inside a `form` element. Without this, an unexpected form submit will happen if it is clicked on a page with a form since the default is `type=submit`.

### 3.1 Submit Buttons

   When a button is needed to submit a form, `<button>` should be the tag used with a `type='submit'`.

```html
<button class='primaryButton' type='submit'>Submit</button>
```
### 3.2 Links
 When a link to another page needs to be styled like a button, use the `<a>` tag like you normally would.

```html
<a href='#' class='primaryButton'>Link</a>
```

### 3.3 Buttons

 If the button doesn't fit any of the scenarios above, it should just simply be a  `<button type='button'>` tag.

```html
<button class='primaryButton' type='button'>Button</button>
```

## Forms

Related `input`s should be grouped in a `fieldset`.
 > This helps semantically group related questions and style questions like checkboxes and radios without the need for a `div.checkboxgroup`.

Each `fieldset` should always have a legend to be HTML valid and meet WCAG standards.

```html
<!-- Bad -->
<form>
    <p>The best Beatle is:</p>
    <label for="john">John</label>
    <input type="radio" name="bestBeatle" id="john" disabled>
    <label for="ringo">Ringo</label>
    <input type="radio" name="bestBeatle" id="ringo" disabled>
    <label for="geoge">George</label>
    <input type="radio" name="bestBeatle" id="george" disabled>
    <label for="pete">Pete</label>
    <input type="radio" name="bestBeatle" id="pete">
    <label for="paul">Paul</label>
    <input type="radio" name="bestBeatle" id="paul" checked>

    <p>Which artist released the following albums?</p>
    <label for="who">Quadrophenia</label>
    <input id="who">
    <label for="queen">A Night at the Opera</label>
    <input id="queen">
    <label for="beachBoys">Pet Sounds</label>
    <input id="beachBoys">
</form>
```

```html
<!-- Good -->
<form>
    <fieldset>
        <legend>The best Beatle is:</legend>
        <label for="john">John</label>
        <input type="radio" name="bestBeatle" id="john" disabled>
        <label for="ringo">Ringo</label>
        <input type="radio" name="bestBeatle" id="ringo" disabled>
        <label for="geoge">George</label>
        <input type="radio" name="bestBeatle" id="george" disabled>
        <label for="pete">Pete</label>
        <input type="radio" name="bestBeatle" id="pete">
        <label for="paul">Paul</label>
        <input type="radio" name="bestBeatle" id="paul" checked>
    </fieldset>

    <fieldset>
        <legend>Which artist released the following albums?</legend>
        <label for="who">Quadrophenia</label>
        <input id="who">
        <label for="queen">A Night at the Opera</label>
        <input id="queen">
        <label for="beachBoys">Pet Sounds</label>
        <input id="beachBoys">  
    </fieldset>
</form>
```
## Comments
> Why? Because they are handy little helpers!

### 5.1 When Things Get Nesty

 Add comments on closing tags when 3 levels of the same type of tags are used in a row. Start counting from the most nested tag outwards and on the 3rd type of that closing tag begin comments.

```html
<!-- Bad -->
<section class="nestyness">
    <div class='class1'>
        <div class='class2'>
            <div class='class3'>
                <div class='class4'>
                    <p class='classP'>This is paragraph content.</p>
                    <p class='classP'>This is paragraph content.</p>
                </div>
            </div>
        </div>
    </div>
</section>
 ```

```html
<!-- Good -->
<section class="nestyness">
    <div class='class1'>
        <div class='class2'>
            <div class='class3'>
                <div class='class4'>
                    <p class='classP'>This is paragraph content.</p>
                    <p class='classP'>This is paragraph content.</p>
                </div>
            </div>
        </div><!--end class2-->
    </div><!--end class1-->
</section>
```
## Semantics
> Semantics help make the web page easier for people and browsers to understand/use.

### 6.1 `<strong>` vs. `<b>`
`<strong>` - should always be used when bolding text, this is the more semantic tag of the two options. Text should be bold for a reason therefore we should use the semantic tag to highlight it.

### 6.2 `<em>` vs. `<i>`
`<em>` - should always be used when italicizing text, this tag adds emphasis in the semantics. Where as, `<i>` only tells the browser the text is just set off from it's normal prose.

## Do NOT Use
> These elements make it harder to change the look and feel of the elements in the future with only css

*DO NOT* use the following elements when writing HTML:

- The break tag -  `<br>` or `<br />`
- The linerule (or thematic change) -  `<hr>` or `<hr />`
- The non-breaking space - `&nbsp;`
