- What is hoisting?
    - "Hoisting" is not actually a thing that happens in the JavaScript engine, it's a conceptual model
    - JavaScript does not actually look ahead at runtime and find all the `var` keywords and `function` keywords and rearrange the code to bring them to the top of the scope
    - What actually happens is that the parser goes through the file and finds all the identifiers at compile/parse time and creates references to them
    - When the scope they belong to is created, variables declared with the `var` keyword are **initialized** with the value of `undefined`, while `let` and `const` variables are **not** (Temporal Dead Zone, they are **declared** but not **initialized**)
- Why aren't function identifiers `undefined` like identifiers declared with `var`?
    - Function **declarations** include the function body as part of the declaration of the identifier, so they are accessible to be run before they are declared lexically
    - Function **expressions** are **not**, because they are not part of the declaration of the identifier. They are only assigned to that identifier when that line of code is executed.
- Hoisting examples:  
    ```js
    introduce(); // "My name is Eileen"  
    sayHello(); // TypeError  
      
    function introduce() { // function declaration  
    console.log("My name is Eileen");  
    }  
      
    var sayHello = function() { // function expression  
    console.log("Hello");  
    }  
      
    console.log(foo); // undefined  
    console.log(baz); // TDZ Error  
      
    var foo = "bar";  
    let baz = "asdf":```
- Undeclared vs. Undefined vs. Uninitialized (TDZ)
    - **undeclared** means that a varaiable was never even declared
        - if you try to access an undeclared variable, you will get a reference error
    - **undefined** means that a variable was declared and assigned value of `undefined`
    - **uninitialized** means that a variable was declared but never assigned a value
        - [Temporal Dead Zone](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#The_temporal_dead_zone_and_typeof)