# Ember

1. **[General Structure](#general-structure)** 
	[ [Properties](#general-structure--properties) ]
1. **[Destructuring](#destructuring)** 
	[ [Objects](#destructuring--objects) ] 
	[ [Get/Set](#destructuring--get-set) ]
1. **[Data](#data)**
	[ [Data Binding](#data--data-binding) ]
1. **[Computed Properties](#computed-properties)**
  [ [Brace Expansion](#computed-properties--brace-expansion) ]
1. **[CSS](#css)**
	[ [Usage](#css--usage) ]
1. **[Actions](#actions)**
	[ [Location](#actions--location) ]
1. **[Error Handling](#errorHandling)** 
	[ [ Overall Application Errors](#errorHandling--overallApplication) ]
1. **[Testing](#testing)**
  [ [Test Scripts](#testing--test-scripts) ]
  [ [Unit Tests](#testing--unit-tests) ]
1. **[Definition of Ready](#deployment-checklist)**
1. **[Addons](#addons)**
  [ [Versioning](#versioning) ]

<a name="general-structure"></a>

## General Structure

**Why do we structure our Javascript code?**
This allows us to follow a consistent coding conventions. Paired with a tool to enforce code structure, such as Eslint, it will provide a way for us to make sure that our code follow a specific specified order.

<a name="general-structure--properties"></a>

### 1.1 Model Property Order

1. Attributes
1. Relations
1. Single line computed properties
1. Multi line computed properties
1. Other structures (custom methods etc.)

```javascript
import DS from 'ember-data';
const { Model, attr, hasMany } = DS;
import { computed, get } from '@ember/object';

// bad
export default Model.extend({
  mood: computed('health', 'hunger', function() {
    const result = get(this, 'health') * get(this, 'hunger');
    return result;
  }),

  hat: attr('string'),

  behaviors: hasMany('behaviour'),

  shape: attr('string')
});

// good
export default Model.extend({
  // 1. Attributes
  shape: attr('string'),

  // 2. Relations
  behaviors: hasMany('behaviour'),

  // 3. Computed Properties
  mood: computed('health', 'hunger', function() {
    const result = get(this, 'health') * get(this, 'hunger');
    return result;
  })
});
```
Enforce by adding this rule to `.eslint`:
```json
rules: {
    'ember/order-in-models': 2
}
```

### 1.2: Controller Property Order

1. Controller injections
1. Service injections
1. Query params
1. Default controller's properties
1. Custom properties
1. Single line computed properties
1. Multi line computed properties
1. Observers
1. Actions
1. Custom / private methods

```javascript
import Controller from '@ember/controller';
import { computed, get } from '@ember/object';
import { inject as service } from '@ember/service';
import { inject as controller } from '@ember/controller';

export default Controller.extend({
  // 1. Controller injections
  application: controller(),

  // 2. Service injections
  currentUser: service(),

  // 3. Query params
  queryParams: ['view'],

  // 4. Default controller's properties
  concatenatedProperties: ['concatenatedProperty'],

  // 5. Custom properties
  attitude: 10,

  // 6. Single line Computed Property
  health: alias('model.health'),

  // 7. Multiline Computed Property
  levelOfHappiness: computed('attitude', 'health', function() {
    return get(this, 'attitude') * get(this, 'health') * Math.random();
  }),

  // 8. Observers
  onVahicleChange: observer('vehicle', function() {
    // observer logic
  }),

  // 9. All actions
  actions: {
    sneakyAction() {
      return this._secretMethod();
    },
  },

  // 10. Custom / private methods
  _secretMethod() {
    // custom secret method logic
  },
});
```
Enforce by adding this rule to `.eslint`:
```json
rules: {
    'ember/order-in-controllers': 2
}
```

### 1.3: Component Property Order

1. Services
1. Default values
1. Single line computed properties
1. Multiline computed properties
1. Observers
1. Lifecycle Hooks (in execution order)
1. Actions
1. Custom / private methods

```javascript
import Component from '@ember/component';
import { computed, get } from '@ember/object';
import { alias } from '@ember/object/computed';
import { inject as service } from '@ember/service';

export default Component.extend({
  // 1. Services
  i18n: service(),

  // 2. Properties
  role: 'sloth',

  // 3. Empty methods
  onRoleChange() {},

  // 4. Single line Computed Property
  vehicle: alias('car'),

  // 5. Multiline Computed Property
  levelOfHappiness: computed('attitude', 'health', function() {
    const result = get(this, 'attitude') * get(this, 'health') * Math.random();
    return result;
  }),

  // 6. Observers
  onVehicleChange: observer('vehicle', function() {
    // observer logic
  }),

  // 7. Lifecycle Hooks
  init() {
    // custom init logic
  },

  didInsertElement() {
    // custom didInsertElement logic
  },

  willDestroyElement() {
    // custom willDestroyElement logic
  },

  // 8. All actions
  actions: {
    sneakyAction() {
      return this._secretMethod();
    }
  },

  // 9. Custom / private methods
  _secretMethod() {
    // custom secret method logic
  }
});
```
Enforce by adding this rule to `.eslint`:
```json
rules: {
    'ember/order-in-components': 2
}
```

### 1.4: Route Property Order

1. Services
1. Default route's properties
1. Custom properties
1. beforeModel() hook
1. model() hook
1. afterModel() hook
1. Other lifecycle hooks in execution order (serialize, redirect, etc)
1. Actions
1. Custom / private methods

```javascript
import Route from '@ember/routing/route';
import { get, set } from '@ember/object';
import { inject as service } from '@ember/service';

export default Route.extend({
  // 1. Services
  currentUser: service(),

  // 2. Default route's properties
  queryParams: {
    sortBy: { refreshModel: true },
  },

  // 3. Custom properties
  customProp: 'test',

  // 4. beforeModel hook
  beforeModel() {
    if (!get(this, 'currentUser.isAdmin')) {
      this.transitionTo('index');
    }
  },
  
  // 5. model hook
  model() {
    return get(this, 'store').findAll('article');
  },
  
  // 6. afterModel hook
  afterModel(articles) {
    articles.forEach((article) => {
      set(article, 'foo', 'bar');
    });
  },

  // 7. Other route's methods
  setupController(controller) {
    set(controller, 'foo', 'bar');
  },

  // 8. All actions
  actions: {
    sneakyAction() {
      return this._secretMethod();
    },
  },

  // 9. Custom / private methods
  _secretMethod() {
    // custom secret method logic
  },
});
```
Enforce by adding this rule to `.eslint`:
```json
rules: {
    'ember/order-in-routes': 2
}
```

### 1.5: Default Unassigned Property

- When declaring an unassigned property we should default it to `null` instead of `undefined`.
- Why? Because defining a property as `undefined` has the meaning of we didn't define it. By setting it to `null` you are explicitly telling the app that the property has been declared.
  
```Javascript
// bad
foo: undefined

// good
foo: null
```

<a name="destructuring"></a>
## Destructuring
Extract multiple values from data stored in objects and arrays.
> Why? Destructuring allows you to import only which classes you need and then extract (or destructure) only the properties that you need. This makes our modules more efficient.

<a name="destructuring--objects"></a>
### 2.1 Destructuring Objects
In Ember 2.16 and above the recommended way to access framework code in Ember applications is via the JavaScript modules API. This makes Ember feel less overwhelming to new users and start faster. These effects are felt because of replacing the Ember global with a first-class system for importing just the parts of the framework you need.
JavaScript modules make the framework easier to document, make the distinction between public and private API much easier to maintain, and provide opportunities for performance work such as tree-shaking.

```javascript
//Bad
import Ember from 'ember';

export default Ember.Component.extend({
    session: Ember.inject.service(),
    title: 'ASH Development'
});

//Good
import Component from '@ember/component';
import { inject as service } from '@ember/service';

export default Component.extend({
    session: service(),
    title: 'ASH Development'
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
import { get, set } from '@ember/object';

set(this, 'isDestructured', true);
get(this, 'isDestructured'); //true

set(someObject, 'isUpdated', true);
get(someObject, 'isUpdated'); //true
```


<a name="data"></a>
## Data

<a name="data--data-binding"></a>
### 3.1 Data Binding

Any new code written by ASH should only contain one-way data-binding. Third-party addons using two-way data-binding is ok, but be cautious and conscious of the side effects it can have.

> Why? Two-way data-binding can have unexpected side effects; data flowing one way keeps things more predictable. For example: if you pass the same data to 2 components and they both are allowed to change it. DDAU allows the parent to arbitrate those changes and resolve conflicts. Otherwise component A can make changes you are not expecting in component B. This is not just a matter of 2 compoents, you can make a change to a child that propagates through the whole app.

```hbs
{{!--Bad--}}
{{!--parent-component.hbs--}}
{{my-cheesy-component 
    myData = model
}}
```
```Javascript
//my-cheesy-component.js
//The below code would update the parent and any other component the parentData object was passed to
set(this, 'model.cheese', 'gouda')
```
```hbs
{{!--Good--}}
{{!--parent-component.hbs--}}
{{my-cheesy-component 
    myData = model
    changeCheese = (action 'newCheesePlease')
}}
```
```Javascript
//my-cheesy-component.js
actions: {
    quesoChanger(e) {
        e.preventDefault();
        get(this, 'changeCheese')(get(this, 'cheese'));
    }
}
```

```hbs
{{!--my-cheesy-component.hbs--}}
<form onSubmit=(action 'quesoChanger')>
    <input type='text' value={{cheese}} onChange={{action (mut cheese) value="target.value"}}>
    <button type='submit'>Change Cheese</button>
</form>
```
```Javascript
//parent component.js, router, or controller
actions: {
    newCheesePlease(cheeseType){
        set(this, 'model.cheese', cheeseType);
    }
}
```
*Twiddle Demo: <a href="https://ember-twiddle.com/386c86a1ad7fa9d15e9cc1f699e5f539">click here</a>*

<a name="computed-properties"></a>
## Computed Properties

<a name="computed-properties--brace-expansion"></a>
### 4.1 Brace Expansion

When a computed property depends on multiple properties of the same object, specify the properties using brace expansion.
> Why? Using brace expansion in computed properties makes our code more organized and easier to read, as it organizes dependent keys. 

```javascript 
// Bad 
fullname: computed('user.firstname', 'user.lastname', function() {
    const { firstname, lastname } = get(this, 'user');

    return `${firstname} ${lastname}`;
})

// Good
fullname: computed('user.{firstname,lastname}', function() {
    const { firstname, lastname } = get(this, 'user');

    return `${firstname} ${lastname}`;
})
```


<a name="css"></a>
## CSS

<a name="css--usage"></a>
### 5.1 Usage
CSS is permitted (and encouraged) in apps and addons under certain circumstances

> Why? Flow, interaction and breakpoints generally belong to the component and not the domain (host site). Properties such as colors, fonts styles, etc. should belong to host site, so that each site can have its own identity. Moving CSS into component files will also cut down on the size of domain CSS bundles and help mitigate the issue of shipping a lot of CSS that belongs to components not in use on that site.


#### Properties Allowed:
- Box Model
 - e.g., `padding`, `height`, `margin`, `border-width`
- Display
 - e.g., `display:flex`, `flex-direction: column`
- Animations and Transitions
- Certain Font Styles
 - `font-weight` only


**Use discretion when adding colors to apps/addons**
* Neutral/non-branding colors that aren't likely to change across websites are OKAY to add to that app's/addon's app styles
* Talk to UX if there is any ambiguity

### Examples of Properties to NOT use
**Site-specific Colors**
 - Do not add site specific colors or site specific variables that are not resuable

**Text styles**
 - e.g., `line-height`, `font-family` these are set on a per site basis

```scss
// Bad
// app/style/appName.scss
.chats {
    padding: 1rem;
    display: flex;
    background: $color1;

    .chatMsg {
        font-family: $font1;
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
        font-family: $font1;
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
### 6.1 Location

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
### 7.1 Overall Application Errors
Every app should contain a base error function within the application route.
> Why? Developers are not always perfect, this will insure that even missed errors from other components, controllers or routes will be handled at the application level. Uncaught errors lead to a bad user experiences.


```javascript
//Example code for route/application.js
import Route from '@ember/routing/route';
import { get, set } from '@ember/object';

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
            } else if (error && error.message) {
                //if it has an error.message log the message to the console for debugging
                console.error(error.message);
                //if it is not a string set the errorToShow property to the genericError
                set(this, 'errorToShow', get(this, 'genericError'))
            } else {
                //if it is not a string set the errorToShow property to the genericError
                set(this, 'errorToShow', get(this, 'genericError'))
            }

            //adds the error to the app container for users to see error message, so they are not left with a blank app container
            app.innerHTML = `<div class='error'>${get(this, 'errorToShow')}</div>`
        }
    }
});
```
<a name="testing"></a>
## Testing

<a name="testing--test-scripts"></a>
### 8.1 Test Scripts

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

<a name='testing--unit-tests'></a>
### 8.2 Unit Tests

> Why? Unit tests are the most basic test for testing the core functionality of the app and relying on integration and acceptance tests can provide a false sense of code coverage.

All **Ember.computed** properties and **_private** methods should be unit tested and have 100% **branch coverage**.

**Branch Coverage** means that each branch in a program (e.g., loops, if/else statements), is executed at least once when tests are run. Having 100% branch coverage ensures that all reachable code is executed which in turn helps developers catch any potential issues and address possible corner cases.

Example of good and bad test cases:
```javascript
// my-component.js
_isLocal(location) {
  if (location === 'SD') {
    return true;
  } else {
    return false;
  }
}

// Examples of tests given private _isLocal() function defined above:

// BAD test case - only catches one branch
assert.equal(this._isLocal('CSC'), false, 'CSC office is not local');

// GOOD test case - provides 100% branch coverage, as it covers both branches
assert.equal(this._isLocal('CSC'), false, 'CSC office is not local');
assert.equal(this._isLocal('SD'), true, 'SD office is local');
// also provide test cases for other potential values
assert.equal(this._isLocal(), false, 'No location provided should still return false (not local)');
assert.equal(this._isLocal(null), false, 'Unexpected value like null or undefined should also return false (not local)');

```


<a name="deployment-checklist"></a>
## Definition of Ready

### 1. Linted
As of __Ember CLI 2.12__, ember comes installed with `ember-cli-eslint`. This will output lint errors in the local server command line, as well as display errors in unit, integration, and acceptance tests. 

### 2. Loading Indicators
Any content that can be updated should have a loading indicator.  
Use the `ash-loader` addon for this.

### 3. 404 template
A scenario for when the API returns a server error should be considered. Create a 404 template using the `ash-four-oh-four` addon.

### 4. Catch Errors
Determine where the app could break and catch errors to keep the user informed.

### 5. unit/acceptance/integration tested
As new logic is added to the app, the appropriate tests should be set up to ensure that future updates don't interfere with your current work.

### 6. Code Coverage
Be sure to utilize the `ember-cli-code-coverage` addon and set up the appropriate npm tests as outlined above.

*ASH Front End Principles denote that branch coverage should be a minimum of 75% total test coverage, and 50% for tests not including acceptance tests.*

### 7. Accessibility Tested
`ember-a11y-testing` should be installed, configured, and added to unit, integration, and acceptance tests.

### 8. Mirage
If the app makes API calls, `ember-cli-mirage` should be installed and configured to match the real API.

### 9. Cross-Browser Tested
Test the app in every browser that we support.  

If Mirage is being used, and the real API is available, test with both sets of data in each browser.  

*Current Supported Browsers: ie11, firefox, safari (desktop and mobile), and chrome*

### 10. Build Pipelines Defined
Configure **stg.ashui** build with Mirage data  
Configure **production** build with API data  

Create a build definition for the app that will:  
1. set npm registry path
2. run `npm install` or `yarn`
3. run `bower install`
4. run `npm test`
5. run `ember build --environment=preview --output-path=preview`
6. run `ember build --environment=production`
7. copy files to the drop location
8. publish files to stg.ashui and the build location
9. publish Code Coverage results
10. notify the appropriate Slack channels

### 11. Checklist Violations
`npm test` (in the build definition) will catch errors and reject build



## Addons

### 9.1 Versioning

1. We define `1.0.0` release as the first version ready to be used by the end user. It may not be feature complete, but it should bring some benefit to our customers. `1.0.0` release should include tests and be bug free according to the developer. As soon as the package is ready to be used in production, it needs to be on version `1.0.0`.

We follow [Semantic Versioning](https://semver.org/) standards for versioning our addons:
1. **Major** version should be used when making incompatible API changes.

1. **Minor** version should be used when adding new functionality in a backward compatible manner.
> Note: When updating `ember-cli` , use **minor** update.

1. **Patch** version should be used when making backward compatible bug fixes, documentation updates, small enhancements (e.g., performance) that do not add new feature, or dependency updates.

