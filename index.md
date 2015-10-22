---
layout: default
title: JsonLogic
---


## Virtues

  1. **Terse.**
  1. **Consistent.** `{"operator" : ["values" ... ]}`  Always.
  1. **Secure.** We never `eval()`. Rules only have access to data you provide.
  1. **Flexible.** Most operands are 1 line of code.


## Examples

### Simple
```js
jsonLogic( { "==" : [1, 1] } );
// true
```

This is a simple test, equivalent to `1 == 1`.  A few things about the format:

  1. The operator is always in the "key" position. There is only one key per JsonLogic rule.
  1. The values are typically an array.
  1. Each value can be a string, number, boolean, array (non-associative), or null

### Compound
Here we're beginning to nest rules. 

```js
jsonLogic(
	{"and" : [
	  { ">" : [3,1] },
	  { "<" : [1,3] }
	] }
);
// true
```
  
In an infix language (like JavaScript) this could be written as:

```js
( (3 > 1) && (1 < 3) )
```
    
### Data-Driven

Obviously these rules aren't very interesting if they can only take static literal data. Typically `jsonLogic` will be called with a rule object and a data object. You can use the `var` operator to get attributes of the data object:

```js
jsonLogic(
	{ "var" : ["a"] }, // Rule
	{ a : 1, b : 2 }   // Data
);
// 1
```

If you like, we support [syntactic sugar](https://en.wikipedia.org/wiki/Syntactic_sugar) on unary operators to skip the array around values:

```js
jsonLogic(
	{ "var" : "a" },
	{ a : 1, b : 2 }
);
// 1
```

You can also use the `var` operator to access an array by numeric index:

```js
jsonLogic(
	{"var" : 1 },
	[ "apple", "banana", "carrot" ]
);
// "banana"
```

Here's a complex rule that mixes literals and data. The pie isn't ready to eat unless it's cooler than 110 degrees, *and* filled with apples.

```js
var rules = { "and" : [
  {"<" : [ { "var" : "temp" }, 110 ]},
  {"==" : [ { "var" : "pie.filling" }, "apple" ] }
] };

var data = { "temp" : 100, "pie" : { "filling" : "apple" } };

jsonLogic(rules, data);
// true
```

### Always and Never
Sometimes the rule you want to process is "Always" or "Never."  If the first parameter passed to `jsonLogic` is a non-object, non-associative-array, it is returned immediately.

```js
//Always
jsonLogic(true, data_will_be_ignored);
// true

//Never
jsonLogic(false, i_wasnt_even_supposed_to_be_here);
// false
```
    
## What next?

Check out the complete list of [supported operations]({{site.base_url}}/operations.html) or try out your own rules in the [web playground]({{site.base_url}}/play.html).
