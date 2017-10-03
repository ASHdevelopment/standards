# Ember
1. [General Structure](#general-structure)
    - [Properties](#general-structure--properties)
1. [Destructuring](#destructuring)
    - [Objects](#destructuring--objects)
    - [Get/Set](#destructuring--get-set)
1. [CSS](#css)
    - [Usage](#css--usage)
1. [Actions](#actions)
    - [Location](#actions--location)
1. [Error Handling](#errorHandling)
    - [Overall Application Errors](#errorHandling--overallApplication)

<a name="general-structure"></a>
## General Structure

<a name="general-structure--properties"></a>
- 1.1 **Property Order**: For components and controllers, follow this order for properties and methods
  + **Properties**
    + Put ember specific properties (e.g., `classNames`, `tagNames`) before custom properties (e.g., `someSpecificAshProp : true`)
  + Component lifecycle hooks (in order) <a href="https://guides.emberjs.com/v2.12.0/components/the-component-lifecycle/">see order in documentation</a>
  + Custom methods
  + `actions` go last

<a name="destructuring"></a>
## Destructuring
Extract multiple values from data stored in objects and arrays.
> Why? Destructuring allows you to import only which classes you need and then extract (or destructure) only the properties that you need. This makes our modules more efficient.

<a name="destructuring--objects"></a>
### 2.1 Destructuring Objects

```javascript
//Bad
export default DS.Model.extend({
  firstName: DS.attr('string'),
  lastName: DS.attr('string'),
  fullName: Ember.computed('firstName', 'lastName', function() {
    //compute full name
  })
});

//Good
import Ember from 'ember';
import DS from 'ember-data';

const {
  computed
} = Ember;

const {
  Model,
  attr
} = DS;

export default Model.extend({
  firstName: attr('string'),
  lastName: attr('string'),
  fullName: computed('firstName', 'lastName', function() {
    //compute full name
  })
});
```
<a name="destructuring--get-set"></a>
### 2.2 Using `get` and `set`
Destructuring `get` and `set` will allow you pass in a POJO, rather than being limited to just the current object with the `this` keyword.

```javascript
//Bad
this.set('isDestructured', false);
this.get('isDestructured'); //false

//Good
import Ember from 'ember';

const {
  get,
  set
} = Ember;

set(this, 'isDestructured', true);
get(this, 'isDestructured'); //true

set(someObject, 'isUpdated', true);
get(someObject, 'isUpdated'); //true
```
<a name="css"></a>
## CSS

<a name="css--usage"></a>
### 3.1 Usage
CSS is permitted (and encouraged) in apps and addons under certain circumstances

> Why? Flow, interaction and breakpoints generally belong to the component and not the domain (host site). Properties such as colors, fonts styles, etc. should belong to host site, so that each site can have its own identity. Moving CSS into component files will also cut down on the size of domain CSS bundles and help mitigate the issue of shipping a lot of CSS that belongs to components not in use on that site.


#### Properties Allowed:
- Box Model
 - e.g., `padding`, `height`, `margin`, `border-width`
- Display
 - e.g., `display:flex`, `flex-direction: column`
- Animations and Transitions

#### Examples of Properties to Not use
- Colors
 - e.g., `color`, `background`
- Text styles
 - e.g., `font-weight`, `font=family`

```scss
// Bad
// app/style/appName.scss
.chats {
  padding: 1rem;
  display: flex;
  background: $color1;

  .chatMsg {
    font-weight: 700;
    text-transform: uppercase;    
    background: $color2;
    color: $color1;
    transition: all 1s ease-in-out;
  }
}

// Good
// base/social.scss
.chats {
  background: $color1;

  .chatMsg {
    font-weight: 700;
    text-transform: uppercase;    
    background: $color2;
    color: $color1;
  }
}

// app/style/appName.scss
.chats {
  padding: 1rem;
  display: flex;

  .chatMsg {
    transition: all 1s ease-in-out;
  }
}
```
<a name="actions"></a>
## Actions

<a name="actions--location"></a>
### 4.1 Location

 - Form Actions should be placed on the form element

```html
//Bad
<form>
  <input id="firstName" type="text" />
  <input id="lastName" type="text" />
  <button type="submit" {{action 'SubmitForm'}}> Submit</button>
</form>

//Good
<form {{action 'SubmitForm' on='submit'}}>
  <input id="firstName" type="text" />
  <input id="lastName" type="text" />
  <button type="submit"> Submit</button>
</form>

```
- Button actions that are **not** in a `<form>` should be placed on the `<button>` itself

```html
//Bad
<div class="container" {{action 'showHide'}}>
 <button type="submit">Submit</button>
</div>

//Good
<div class="container">
 <button type="submit" {{action 'showHide'}}>Submit</button>
</div>

```
<a name="errorHandling"></a>
## Error Handling
<a name="errorHandling--overallApplication"></a>
### 5.1 Overall Application Errors
Every app should contain a base error function within the application route.
> Why? Developers are not always perfect, this will insure that even missed errors from other components, controllers or routes will be handled at the application level. Uncaught errors lead to a bad user experiences.


```javascript
//Example code for route/application.js
const {
  set,
  get
} = Ember;

export default Route.extend({
  genericError: 'Hmm, something went wrong.',

  actions: {
    error(error){
      //grabbing app container for a place to put the error so it will show to the user
      const app = document.getElementById('app-container');

      //getting the content for the error to show the user
      if(typeof error === 'string') {
        //if its a string set the errorToShow property to that string
        set(this, 'errorToShow', error);
      } else {
        //if it is not a string set the errorToShow property to the genericError
        set(this, 'errorToShow', get(this, 'genericError'))
      }

      //adds the error to the app container for users to see error message, so they are not left with a blank app container
      app.innerHtml = `<div class='error'>${get(this, 'errorToShow')}</div>`
    }
  }
});
```
