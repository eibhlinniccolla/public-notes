- Each data property (object property that store data) has not only the name-value pair, but also 3 attributes (the three attributes are set to true by default):
	- `configurable`
		- Can the property be changed or deleted (via the `delete` operator)?
	- `enumerable`
		- Can the property be returned in a `for`/`in` loop?
	- `writable`
		- Can the value of a property be changed?

You can set these attributes when you add a property to an object by using `Object.defineProperty()`
        - Example:  
            ```  
            var obj = { a: 1 };  
            Object.defineProperty(obj, "b", {  
            value: 2,  
            enumerable: false  
            });  
              
            console.log(obj.a); // 1  
            console.log(obj.b); // 2  
              
            for (var prop in obj) {  
            console.log(prop); // "a"  
            }  
            ```
    - You can access the object containing these attributes by using `Object.getOwnPropertyDescriptor()`
        - Example:  
            ```  
            var obj = { a: 1 };  
              
            console.log(Object.getOwnPropertyDescriptor(obj, "a"));  
            /* {  
            value: 1,  
            writable: true,  
            configurable: true,  
            enumerable: true  
            } */  
              
            ```