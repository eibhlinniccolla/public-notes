- JavaScript organizes scopes with **functions** and **blocks**
    - Block scope
        - you can declare a block scope variable using curly braces and `let`  
            `var myVar = 'Eileen';  
            {  
            let myVar = 'Natasha';  
            console.log(myVar); // 'Natasha'  
            }  
            console.log(myVar); // 'Eileen'  
            `
        - If a variable is only needed for a few lines, it's a good idea to open up a block with curlies so that the variable is contained only to where it is needed
        - Use `var` to declare variables that are intended to be available for the entire function, so that it's clear
        - Blocks are **not** scopes **until** they have a `let` or a `const` inside them, which implicitly turns them into a scope
        - `var` variables can be declared more than once in a function, `let` and `const` variables cannot  
            `function foo () {  
            try {  
            var baz = "bar";  
            } catch (err) {  
            var baz = "foobar"; // totally legit  
            }  
            }`
            - You can use this to make it explicit in a place far from a variable's declaration what scope it belongs to.
        - `let` and `const` guidelines
            - Use them in places where you're already signaling to the user that the variable is intended to be block scoped (e.g. in an `if` statement, in a `for` loop)
            - Only use `const` with immutable (primitive) values
                - constants are meant to be semantic placeholders for values that never change