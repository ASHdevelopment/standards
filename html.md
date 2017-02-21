# HTML

 1. [Attribute Ordering](#attribute-ordering-within-tags)
 1. [Quotes](#quotes)
 1. [Buttons](#buttons)
 1. [Forms](#forms)
 1. [Comments](#comments)
 1. [Do NOT Use](#do-not-use)

## 1. Attribute Ordering within Tags
 > Makes code easier to read and easier for a new developer to jump in and find what they are looking for.

  ### 1.1 Attribute Order
   Ordering from the beginning of the tag to the close of the tag is as follows:

  1. id/for
  1. class
  1. type (eg: radio)
  1. property (eg: value, checked, readonly)
  1. tag specific (eg: href, action)
  1. ARIA/accessibility
  1. data- attributes
  1. KnockOut related attributes (eg: data-bind)

**Bad**
 ```html
 <input type='...' class='...' id='...' .... etc. />
 ```

**Good**
 ```html
 <input id='...' class='...' type='...' ... etc. />
 ```


## 2. Quotes
  > Hint - it's easier to type single quotes.

+ Use whatever is comfortable to you
+ Do NOT go back to change code to suit your style

**Bad**
```html
<div class="class1"> //changing this to single quotes to fit your style
      <p class='class2'>This is paragraph content.</p>
</div>
```

**Good**
```html
<div class='class1'>
      <p class='class2'>This is paragraph content.</p>
</div>

//or

<div class="class1">
      <p class="class2">This is paragraph content.</p>
</div>
```


## 3. Buttons
  > Buttons can submit a form, be a link styled like a button, or simply just a button.

### 3.1 Submit Buttons

   When a button is needed to submit a form, ``<button>`` should be the tag used with a ``type='submit'``.

  ```html
<button class='primaryButton' type='submit'>Submit</button>
  ```
### 3.2 Links
  For a link to another page just needs to be styled like a button, use the ``<a>`` tag like you normally would.

  ```html
  <a href='#' class='primaryButton'>Link</a>
  ```

### 3.3  Button

 If the button doesn't fit any of the scenarios above, it should just simply be a  ``<button>`` tag.

  ```html
<button class='primaryButton' type='button'>Button</button>
```

<a name="forms"></a>
## 4. Forms

Related `input`s should be grouped in a `fieldset`.
 > This helps semantically group related questions and style questions like checkboxes and radios with the need for a `div.checkboxgroup`.

Each `fieldset` should always have a legend to be HTML valid and meet WCAG standards.

**Bad**
```html
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

**Good**
```html
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
    <input id="beachBoys">  </fieldset>
</form>
```

## 5. Comments
  > Comments, why? Because they are handy little helpers!

### 5.1 When Things Get Nesty

 Add comments on closing tags when 3 levels of the same type of tags are used in a row. Start counting from the most nested tag outwards and on the 3rd type of that closing tag begin comments.

 **Bad**
 ```html
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

 **Good**
 ```html
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


## 6. Do NOT Use
  > These elements dictate styles that cannot be changed by CSS making it harder to edit the appearance in the future.

*DO NOT* use the following elements when writing HTML:

 + The break tag -  ``<br>`` or ``<br />``
