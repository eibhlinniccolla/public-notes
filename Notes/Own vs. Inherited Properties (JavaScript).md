- [[Own Property (JavaScript)]]
- [[Inherited Property (JavaScript)]]

**Own** and **Inherited** Properties
- "Own" vs. "Inherited"
- Determing if a property is owned or inherited
	- To determine if a property exists on an object (whether own or inherited), use the `in` operator
		- Example:  
			```  
			var obj = { a: 1 };  
			var b = Symbol();  
			obj[b] = 2;  
			  
			console.log("a" in obj); // true  
			console.log(b in obj); // true  
			console.log("c" in obj); // false  
			console.log("toString" in obj); // true, inherited from Object.prototype  
			```  
			
	- To determine if an object has a specific property as an own property, use `Object.prototype.hasOwnProperty()`
		- Example:  
			```  
			// Create a new school object with a property name schoolName​  
			​var school = { schoolName: "MIT" };  
			​  
			​// Prints true because schoolName is an own property on the school object​  
			console.log(school.hasOwnProperty("schoolName")); // true​  
			  
			​// Prints false because the school object inherited the toString method from Object.prototype, therefore toString is not an own property of the school object.​  
			console.log(school.hasOwnProperty("toString")); // false  
			```  
			
		- Useful when you need to enumerate an object and you only want the own properties and not inherited ones.
- Listing an object's own properties
	- `Object.getOwnPropertyNames()` will return an array of an object's **own**, **enumberable**, `String` properties
	- `Object.getOwnPropertySymbols()` will return an array of an object's **own**, **enumberable**, `Symbol` properties.
	- Example  
		```var obj = { a: "foo" };  
		var sym = Symbol();  
		obj[sym] = "bar";  
		  
		for (var i in obj) {  
		console.log(i); // "a" , symbols don't show up in for...in loops  
		}  
		  
		Object.getOwnPropertyNames(obj); // ['a']  
		Object.getOwnPropertySymbols(obj); // [Symbol()]```
- 