# General Standards at ASH

## Commenting

Comments should be placed on the line above the code that it describes.
> Why? When tracking change history in a version control system by changesets associated with line numbers, it is easier to see when changes were applied to a comment vs actual code.

## Variable Casing

When not overridden by a more specific rule, we use [lowerCamelCase](https://en.wiktionary.org/wiki/lowerCamelCase) for naming. This applies to JavaScript variables and functions, SCSS variables and mixins, HTML attributes (when not dasherized), CSS classes, and anything else that doesn't have its own rule already.
> Why? This reduces the thought required when naming items and produces a consistent code base.

```javascript
//Bad
let dessertsize = 'only wafer thin';

function DistributeCilantro() {

// Good
const yearsOfPower = 1000;

function deprecateAshForm() {

class Rectangle { //ES6 and Ember classes still use UpperCamelCase by general consensus
```
