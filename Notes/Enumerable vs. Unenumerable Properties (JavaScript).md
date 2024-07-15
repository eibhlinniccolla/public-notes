**Enumerable** vs. **Unenumerable** properties
    - the properties of an object can be either of type `String` or `Symbol`
        - Example:  
            ```var sym = Symbol('foo');  
              
            var obj = { [sym]: 1, name: 'Joe' };  
              
            obj[sym]; // 1  
            obj.sym; // undefined  
            obj.name // joe  
            obj["name"] // joe```
    - **Enumerable** properties are properties which show up in a `for ... in` loop, unless the property's key is a `Symbol`
        - Symbols are not enumerable in`for...in` iterations, but they are still enumerable.
    - Enumerability of inherited properties
        - Properties inherited from `Object.prototype` are **not** enumerable (they are not shown in a `for...in` loop)
        - **Inherited** properties that are configured to be enumerable **are** shown, however
        - Example:  
            ```  
            var dog = {  
            legs: 4  
            };  
              
            var corgi = Object.create(dog);  
            corgi.size = "small";  
              
            // corgi inherits .toString from Object.prototype  
            console.log(corgi.toString); // [Function: toString]  
              
            // only props set directly on corgi, and props inherited from dog show up in the for in loop  
            for (var prop in corgi) {  
            console.log(prop); // "size", "legs"  
            }  
            ```