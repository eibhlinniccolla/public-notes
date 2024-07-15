---
published: true
---
- _This_ object contains all the methods callable on **objects**
	- (i.e. `.toString()`, `.create()`, `.hasOwnProperty()`, etc.)
- This object _also_ has a `[[prototype]]` property, but it's set to `null`, since we're at the beginning of the prototype chain

```js
Object.getPrototypeOf(Object.prototype); // null  
```