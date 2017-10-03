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
1. [Updating Ember Apps](#updating-ember-apps)

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

<a name="updating-ember-apps"></a>
## Updating Ember Apps

1. Follow the steps under **Project Update** in the following guide: <https://github.com/ember-cli/ember-cli/releases>

   1. Alternatively, you can use [ember-cli-update](https://github.com/kellyselden/ember-cli-update). However, make sure you are comfortable updating manually before using this method! 

2. When you get to the last step, after running `ember init`, answer **yes** to all the prompts. 
3. Make sure to delete `.jshintrc` files in both the /test and application root directories. Include `.eslintrc` files in the same directories instead. 

   1. Don't forget to include `.eslintrc` files in source control, so it appears in your pending changes. 

4. Work through all the updated files in the app. Ensure that you assess every file and manually merge the necessary updates with the changes that were there before the update.
5. Manually reinstall all dependencies, including ember addons in `package.json`. Use `ember install <addon-name>` for ember addons and `npm install <package-name>` otherwise. **Make sure you are taking the latest minor version of dependencies.**

   1. You will need to go to npm or github for each dependency that was added to the package and assess if it has undergone a `major` update. If not, install the dependency from the command line using the appropriate method. 

   2. **Note:** Make sure that the correct packages make it to the correct `dependencies / devDependencies` block.

6. Once done reinstalling the dependencies, run `rimraf node_modules bower_components` and reinstall them with `npm install` and `bower install`. 

   1. Ideally, there shouldn't be any bower dependencies.

7. At this point you should be able to run the app. If this does not allow the app to run, you probably did not reinstall the dependencies correctly. 
> If you are working on an addon, you will need to update the README, increase the version, and publish to private npm. 
