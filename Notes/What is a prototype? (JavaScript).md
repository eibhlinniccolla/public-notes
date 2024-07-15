---
published: true
---
Essentially a **fallback object** that the JavaScript engine can look at if the requested property or method doesn't exist on an object.

- Example:  
	```  
	var fallback = { increment: function () { obj.score++; } }  
	  
	var obj = Object.create(fallback);  
	obj.name = "Eileen";  
	obj.score = 3;  
	  
	obj.score; // 3  
	obj.increment();  
	obj.score; // 4  
	  
	Object.getPrototypeOf(obj) === fallback; // true  
	```
	- Create an object called `fallback` that has an `increment` method
	- Create a new object called `obj` with `Object.create`, passing in `fallback` as an argument
	- Set the `name` and `score` properties of `obj`
	- When you attempt to call the `increment` method on `obj`, JavaScript doesn't find it, because `obj` has no property called `increment`
	- However, because we created `obj` with `Object.create` and passed in `fallback`, JavaScript knows to go look at `fallback` to find the `increment` property
	- It looks at `fallback`, finds the `increment` property, and so our code works as expected, and the `score` property of `obj` is incremented
	- We can confirm that this is what is happening by getting the prototype of `obj` using `Object.prototype.getPrototypeOf()`, and check that it has an `increment` property