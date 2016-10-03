
# HTML-bars/Handle-bars

## Table of Contents
1. [quotes](#quotes)
1. [properties](#properties)
	- [alignment](#properties--alignment)

## quotes
Double or single quotes
* Flexible, use what's comfortable
* DO NOT go back to change code to fit your styling

**Bad**
N/A

**Good**
```
{{ash-plugin
	message="this is the message"
}}
```

or

```
{{ash-plugin
	message='this is the message'
}}
```
## Properties
<a name="properties--alignment"></a><a name="1.1"></a>
### [1.1](#properties--alignment): alignment
* line properties vertically

**Bad**
```
{{ash-plugin message="this is the message" isNew=true list=[1,4,5,8] foo=(action 'bar')}}
```

**Good**
```
{{ash-plugin
	message="this is the message"
	isNew=true
	list=[1,4,5,8]
	foo=(action 'bar')
}}
```
