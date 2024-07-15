---
published: true
---
Every object (including functions) has an internal, **hidden** property called `[[prototype]]`.
- You can't access the `[[prototype]]` property directly from your code
- It can only be read with `Object.getPrototypeOf(someObject)`.
- The value of `[[prototype]]` can only be set when the object is created, either using `Object.create()` or the `new` keyword
- If you don't set `[[prototype]]` to a specific value with the above methods, it defaults to the prototype of the native function of whatever data type it is
	- i.e. `Object.prototype` if it's an object, `Function.prototype` if it's a function, `Number.prototype` if it's a number, etc.