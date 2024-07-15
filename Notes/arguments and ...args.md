- There are two ways you can collect the arguments of a function
    - Using `arguments`  
        `function (arg1, arg2) { console.log(arguments} } // { '0': someValue, '1': anotherValue }`
    - Using `...args`  
        `function(...args){console.log(args)} // [someValue, anotherValue]`
    - Using `arguments` gives you a pseudo-array object containing the values of the arguments with numerical indices
    - Using `...args` gives you a **real** array which allows you to call array methods
    - Using `...args` also allows you to name as many of the arguments as you want, and collect the rest  
        `function (arg1, ...args) { console.log(arg1, args) }  
        // someValue, [anotherValue, aThirdValue]`
    - Arrow functions **do not** have access to the `arguments` keyword
    - `Array.prototype.slice()` can be used to transform "array-like objects" (such as `arguments`) into arrays  
        `function list() {  
        return Array.prototype.slice.call(arguments);  
        }  
          
        var list1 = list(1, 2, 3); // [1, 2, 3]`
        - it can also be reduced using `[].slice.call(arguments)` instead of `Array.prototype.slice.call`
        - [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice#Array-like_objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice#Array-like_objects)
    - In ES6, this can be accomplished using `Array.from()`