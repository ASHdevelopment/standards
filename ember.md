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
1. [Testing](#testing)
	- [Test Scripts](#testing--test-scripts)

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
<a name="testing"></a>
## Testing

<a name="testing--test-scripts"></a>
### 5.1 Test Scripts

> Why? To allow us to establish and hold to a standard of code coverage in all of our apps with meaningful test writing and the ability to gate deployments when those standards are not met.

We use two devDependencies to compile our test scripts.  
1. `cross-env` - For properly setting the NODE_ENV on Windows test environments
1. `ember-cli-code-coverage` - For testing the percentage of code coverage with Istanbul

The `scripts` section in your __package.json__ file should include the following...

``` javascript
"scripts": {
    "test": "cross-env COVERAGE=true ember test",
    "test-server": "cross-env COVERAGE=true ember test --server"
  }
```
