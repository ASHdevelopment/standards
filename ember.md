# Ember
1. [General Structure](#gstructure)
1. [CSS](#css)
1. [actions](#actions)

## General Structure
<a name="gstructure"></a><a name="1.1"></a>

- [1.1](#gstructure) **Property Order**: For components and controllers, follow this order for properties and methods
  + **Properties**
    + Put ember specific properties (e.g., `classNames`, `tagNames`) before custom properties (e.g., `someSpecificAshProp : true`)
  + Component lifecycle hooks (in order) <a href="https://guides.emberjs.com/v2.7.0/components/the-component-lifecycle/">see order in documentation</a>
  + Custom methods
  + `actions` go last


## CSS
<a name="css"></a>

<a name="2.1"></a>
### Usage
CSS is permitted (and encouraged) in apps and addons under certain circumstances

### Why
> Flow, interaction and breakpoints generally belong to the component and not the domain (host site). Properties such as colors, fonts styles, etc. should belong to host site, so that each site can have its own identity. Moving CSS into component files will also cut down on the size of domain CSS bundles and help mitigate the issue of shipping a lot of CSS that belongs to components not in use on that site.

### Properties Allowed:
- Box Model
 - e.g., `padding`, `height`, `margin`, `border-width`
- Display
 - e.g., `display:flex`, `flex-direction: column`
- Animations and Transitions

### Examples of Properties to Not use
- Colors
 - e.g., `color`, `background`
- Text styles
 - e.g., `font-weight`, `font=family`

```scss
// BAD
// base/social.scss
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

// GOOD
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

## Actions
<a name="actions"></a>

### Location

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
<div class="container" {{action 'SubmitForm'}}>
 <button type="submit"> Submit</button>
</div>

//Good
<div class="container">
 <button {{action 'SubmitForm'}} type="submit"> Submit</button>
</div>

```
