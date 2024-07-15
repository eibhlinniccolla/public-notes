---
published: true
---
- Since [[All functions are objects (JavaScript)]], this means they have a `[[prototype]]` ([[prototype (double brackets)]]) property which links to the `Function` function's `prototype` object
- This object contains all the methods callable on functions
	- (i.e. `.call()`, `.bind()`, `.apply()`, etc.)
- This property ALSO has a `[[prototype]]` property which links to the `Object` function's `prototype` object ([[Object.prototype]])

```js
function foo () {}  
  
var fooProto = Object.getPrototypeOf(foo); // Function.prototype  
var functionProto = Object.getPrototypeOf(fooProto); // Object.prototype  
```