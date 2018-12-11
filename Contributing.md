# Document Title 

## Table of Contents 
1. **[Section](#section)**
1. **[Section with Subsections](#section-with-subsections)**
	[ [First Subsection](#21-first-subsection) ] 
	[ [Second Subsection](#22-second-subsection) ]
1. **[Code Examples](#code-examples)**
	[ [JavaScript](#31-javascript) ] 
	[ [Handlebars](#32-handlebars) ]
	[ [CSS](#33-css) ]

	*Example:*  
- Primary section links should be in bold
- Basic markdown links can be linked to the dasherized name of the section title _(links are not case-sensitive, so to save keystrokes and for consistency, use lowercase)_
	```markdown
	1. **[Section](#section)**

	## Section
	```
- Subsection links should be inline with their parent section link and contained in square brackets with surrounding spaces `[ ]`
- Markdown links to sections with floating point numbers in the title can be written in the dasherized format of the title, omitting special characters such as decimals and colons
	```markdown
	2. **[Section with Subsections](#section-with-subsections)**
	  [ [First Subsection](#21-first-subsection) ] 
	  [ [Second Subsection](#22-second-subsection) ]
	
	## Section with Subsections
	
	### 2.1: First Subsection
	
	### 2.2: Second Subsection
	```


## Section
Provide overall description of this section.
> Why? When description is present it is easier for others to understand what the section is about. 

## Section with Subsections


### 2.1: First Subsection


### 2.2: Second Subsection


## Code Examples

### 3.1: JavaScript

When writing code examples, use four spaces for indentation when needed.
> Why? Within the context of code examples, indentation with four spaces makes code blocks easier to read.

```javascript 
// Provide Bad example


// Provide Good example
```

### 3.2: Handlebars

```handlebars 
<!-- Provide Bad example -->


<!-- Provide Good example -->
```

### 3.3: CSS

```css 
/* Provide Bad example */


/* Provide Good example */
```