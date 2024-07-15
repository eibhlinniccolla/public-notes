---
published: true
---
- ` .prototype`
	- Because functions are objects, you can store data in them  
		```function myFunk() {}  
		myFunk.foo = 'bar';  
		console.log(myFunk.foo); // bar```
	- Consequently, all functions also have a hidden `[[prototype]]` property  
		```  
		function myFunk () {}  
		  
		Object.getPrototypeOf(myFunk); // Function.prototype  
		```
	- Functions, in addition to the hidden `[[prototype]]` property, also have a property called `prototype`, and it is this that you can access, and modify, to provide inherited properties and methods for the objects you make.
	- This property automatically contains an empty object, which allows us to add properties and methods to it