# Javascript

1. [Variables](#variables)
1. [Strings](#strings)
1. [Operators](#operators)
1. [Arrays](#arrays)
1. [Objects](#objects)
1. [Functions](#functions)
1. [Iteration](#iteration)
1. [Try/Catch](#trycatch)
1. [Libraries](#libraries)
1. [NPM](#npm)
1. [Documentation & Style](#documentation)

## <a name="variables">Variables</a>

### 1.1 `const/let/var`
#### When working in ES6 (Ember Projects, gulpfile, etc.)

Default to `const`. If you need to change the value later on, then you can use `let`. Do not use `var` **ever**

> This ensures that you can't reassign your references, which can lead to bugs and difficult to comprehend code.

#### When in legacy code (non-Ember projects)
Do **not** use `const` or `let`.

### 1.2 Instantiate variables at top of functions

> Avoids issues with hoisting and temporal dead zones

```javascript
//bad
function foo() {
	this.year = 1993;

	const foo = 'bar';

	this.baz = foo;
}

//good
function foo() {
	const foo = 'bar';

	this.year = 1993;
	this.baz = foo;
}
```

### 1.3 Declare variables across several lines

> Simplify changeset diffs, reduce risk of creating global variables, helps with breakpoints.

```javascript
//very bad
function foo() {
	const foo = 1, bar = 2, baz = 3;
}

//bad
function foo() {
	const foo = 1,
		  bar = 2,
		  baz = 3;
}

//good
function foo() {
	const foo = 'bar';
	const bar = 2;
	const baz = 3;
}
```

### 1.4 Group together `const` and `let` declarations
> To avoid complications...

```javascript
//bad
function sheNever() {
	const foo = 1;
	let bar = 2;
	const baz = 3;
	let bae = 4;
}

//good
function keptTheSameAddress() {
	const foo = 1;
	const baz = 3;
	let bar = 2;
	let bae = 4;
}
```

### 1.5 Variable Casing
When using `const`, do *not* capitalize the variable name.

> It's unnecessary.


```javascript
//bad
const FOO = 1;
const BAR = 3;

//good
const foo = 1;
const baz = 3;
```

## <a name="strings">Strings</a>

### 2.1 String Literals (ES6 only)
> This ensures readability in strings where variables are inserted

Always use string literals when concatenating strings together.

```javascript
//bad
const string = 'The ASH UI team is ' + awesomeAdjective + '!';

//good
const string = `The ASH UI team is ${awesomeAdjective}!`;
```

## <a name="operators">Operators</a>

### 3.1 Equality Operators
Default to Strict Equality, `===`. Use `==` only for special cases, such as when you don't care about the type. When using `==` comment code explaining your choice.

```javascript
//bad
if(1 == '1')        // evaluates to true  

//good
if('5' === 5)       // evaluates to false
if(5 === 5)         // evaluates to true
```

## <a name="arrays">Arrays</a>

### 4.1 Dangling Commas
There is no hard standard for dangling commas.

> Yes it looks ugly and weird but it has benefits in simplifying diffs.

```javascript
//ok
const arr = [
	'foo',
	'bar',
];
const obj = {
	hey: true,
	jude: true,
};

//also ok
const arr = [
	'foo',
	'bar'
];
const obj = {
	hey: true,
	jude: true
};
```

## <a name="objects">Objects</a>

### 5.1 Method Declarations
**When able to code in ES6**, use shorthand method declarations. Otherwise normal method declarations are OK.

```javascript
//bad unless stuck in ES5
const obj = {
    doThisThing: function() {
        return true;
    }
};

//good (but only in ES6)
const obj = {
    doThisThing() {
        return true;
    }
};
```

### 5.2 Dangling Commas
There is no hard standard for dangling commas.

> Yes it looks ugly and weird but it has benefits in simplifying diffs.

```javascript
//ok
const arr = [
	'foo',
	'bar',
];
const obj = {
	hey: true,
	jude: true,
};

//also ok
const arr = [
	'foo',
	'bar'
];
const obj = {
	hey: true,
	jude: true
};
```

## <a name="functions">Functions</a>

### 6.1 Arrow Functions
#### You can only use these within ES6. Ignore this section when working with ES5.

Arrow functions cut down and have special properties in terms of lexical scoping.

This preserves the value of `this` which removes the need for you to "save" the value of this (commonly as `that` or `self`).

```javascript
const myObject = {
	init() {
		const that = this;

		someFunctionWithCallback(1, function() {
			console.log(this);

			//logs the Window object. Most likely not the behavior you are expecting. You have use the "that" variable we initiated up there.

			console.log(that);
		});
	},
	init2() {
		someFunctionWithCallback(1, () => {
			console.log(this);

			//logs myObject!
		});
	}
}
```
When there is only one parameter, do not encase it in parenthesis.

```javascript

//bad
let itemsLength = items.map((item)=>{
 return item.length;
});

//good
let itemsLength = items.map( item =>{
 return item.length;
});
```

#### Exceptions
Use normal syntax for things like event listeners where do **not** want to preserve `this`.

```javascript
// This is a special exception.
//bad
thing.on('click', e => {
	$(this).addClass('newClass');
	//this will NOT work
});

//good
thing.on('click', function(e) {
	$(this).addClass('newClass');
});
```

## <a name="iteration">Iteration</a>

### 7.1 Use of `for` loop vs. iterative methods (`.map()`, `.forEach()`, etc.)

**When working with large datasets**, use the classic `for` loop as it allows you to `break;` out of the iteration, which prevents the expense of looping through unnecessary items after a certain criteria has been met. **You cannot do this with iterative methods**

```javascript
const array = [1, 2, 3, ... 10,000];
const newArray = [];

// Bad
array.forEach((item, index) => {
	if (index < 50) {
		newArray.push(item * 3);
	}
});

// Good
for (let i = 0; i < array.length; i++) {
	if (i === 50) {
		break;
	}
	newArray.push(array[i] * 3);
}
```

## <a name="json">JSON</a>

## <a name="#trycatch">Try/Catch</a>

### 8.1 Try...Catch Basics
> Helps catch errors before they cause the code to blow up

There are 3 clauses that can be used when writing a try...catch statement:

1. try - contains statments to be executed
1. catch - contains the statements to be executed if an exception is thrown in th try block
1. finally - contains statements that are exectured after the try statement is complete regardless of whether or not an exception was thrown or caught

Each try block must have at least one try and one catch clause or a finally clause, or both.

```javascript
try {
    myroutine(); // may throw three types of exceptions
} catch (e) {
    if (e instanceof TypeError) {
        // statements to handle TypeError exceptions
    } else if (e instanceof RangeError) {
        // statements to handle RangeError exceptions
    } else if (e instanceof EvalError) {
        // statements to handle EvalError exceptions
    } else {
       // statements to handle any unspecified exceptions
       logMyErrors(e); // pass exception object to error handler
    }
}
```
## <a name="#trycatch">Try/Catch</a>
### 8.2 Try...Catch for Nesting

Try/Catch should always be used when accessing deeply nested objects, when you cannot guarantee the parent object will be there.

## <a name="#libraries">Libraries</a>

### 9.1 jQuery

#### Use in all QUnit tests
> Why? It is quicker to write unit tests in jQuery, unit tests are not production code, and besides, QUnit was developed by the jQuery team. So why not?

#### Use $.ajax() rather than XMLHttpRequest() for server requests
> Why? The jquery AJAX wrapper is much simpler to write and the speed hit is minute compared to an entire project written in jQuery

**Note:** AJAX Calls in non-Ember projects should utilize `$$ash.ajax`.

#### Write in Vanilla JavaScript wherever possible
> Why? jQuery is a huge library, and no longer necessary for cross-browser compatibility. Writing in vanilla javascript will also give you a deeper understanding of the core language

**Note:** When updating legacy code to use vanilla javascript, be mindful of which objects are passed to 3rd party plugins or in-house plugins that still use jQuery, and wrap the object in a jQuery wrapper accordingly

## <a name="npm">NPM</a>
### 9.1 Semantic Versioning

- Update the Major version when making a breaking API changes.
- Update the Minor version when introducing a new feature.
- Update the Patch version when adding a bug fix.
- Always start on a minor version 0.1.0.
- Do not publish to the registry until the code can deliver functionality, tested, and can be consumed by other applications.
- If the code is being used on production, then it should be at least on version 1.0.0.

## <a name="documentation">Documentation & Style</a>
> Why? Because other devs need to be able to work in your code without setting up a meeting first.

### 10.1 Documentation
While there isn't a hard standard, a good rule of thumb is if you need to explain what it's doing to the next developer, you should add documentation. You can document a block at the top of a function or tell a story as developers step through your code. Don't document obvious things.

```javascript
//BAD

function isPalindrome(word){

	// create and set the newWord variable
	let newWord = word.replace(/[^a-zA-Z0-9]/g, '');

	newWord = newWord.toLowerCase();

	// return true if one of the conditions are true
	if(newWord.length == 0 || newWord.length == 1){
		return true;
	}


	if(newWord === newWord.split('').reverse('').join('')){
		return true;
	}

	// return false
	return false;
}
```

```javascript
// Good

// Check if a given word/string is palindrome
function isPalindrome(word){

	// strip all characters and symbols except a-z, A-Z, and 0-9
	let newWord = word.replace(/[^a-zA-Z0-9]/g, '');

	newWord = newWord.toLowerCase();

	if(newWord.length === 0 || newWord.length == 1){
		return true;
	}

	// check the original word against the new reversed string
	if(newWord == newWord.split('').reverse('').join('')){
		return true;
	}

	return false;

}
```


### 10.2 Readability

- Good code is readable by other developers.
- Good code can do complex things.
- Code that can do complex things, but is not readable by other developers is not good code.
