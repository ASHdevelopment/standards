
# HTML-bars/Handle-bars

## Table of Contents
1. [Quotes](#quotes)
1. [Properties](#properties)
	- [Alignment](#properties--alignment)
1. [Comments](#comments)

## Quotes
> Double or single quotes
* Flexible, use what's comfortable
* DO NOT go back to change code to fit your styling

**Bad**
N/A

**Good**
```handlebars
{{ash-plugin
	message="this is the message"
}}
```

or

```handlebars
{{ash-plugin
	message='this is the message'
}}
```
## Properties
<a name="properties--alignment"></a><a name="1.1"></a>
### [2.1](#properties--alignment): Alignment
> Line properties vertically

**Bad**
```handlebars
{{ash-plugin message="this is the message" isNew=true list=[1,4,5,8] foo=(action 'bar')}}
```

**Good**
```handlebars
{{ash-plugin
	message="this is the message"
	isNew=true
	list=[1,4,5,8]
	foo=(action 'bar')
}}
```
## Comments
* Only acceptable comment structure is curly brackets with exclaimation point after the opening curly baracket.

**Bad**
```handlebars
<!-- this is NOT an appropriate handlebars comment -->
```

**Good**
```handlebars
{{! this IS and appropriate handlebars comment }}
```

