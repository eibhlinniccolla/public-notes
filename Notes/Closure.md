- What is closure
    - When a function is able to remember and access its lexical scope and the variables within it, even when that function executes in a different scope.
    - If a function references a variable in an outside scope, and then that inner function is called at a later time, it keeps the outer scope alive so that it can reference the variable.
    - Technically, **closure** does NOT refer to the _concept_ of a function retaining access to variables defined in an outer function, but people do refer to it that way colloquially
- `[[scope]]`
    - When a function is defined, it gets a `[[scope]]` property which references the Local Variable Environment in which it was defined
    - You don't have direct access to `[[scope]]`
    - You can only access variables in `[[scope]]` within the body of the function where they are referenced
        - You can reference them from outside by returning them from the inner function and storing them in a variable
    - **Closure** is technically the term for `[[scope]]`, i.e. the variable environment which the returned function retains access to
        - Another term is the **Closed Over Variable Environment**
        - Another term is **Lexical Scope Reference**
    - Functions declared in the **global** scope _also_ have a lexical scope reference / closure, a reference to the global scope, and all the variables in it.
- Variables declared in the outer function that are not referenced in the body of the inner function are garbage collected when the function returns  
    `function outer () {  
    const counter = 0; // NOT garbage collected  
    const myVar = 'some value'; // IS garbage collected  
    function inner() {  
    counter ++;  
    }  
    return inner;  
    }`  
    
- Module pattern
    - The "namespace" pattern is not a module because it lacks the idea of **encapsulation**  
        `var myNamespace = {  
        name: 'Eileen',  
        sayHello() {  
        console.log('Hello ' + this.name);  
        }  
        }` // namespace, not a module
    - Modules encapsulate data and behavior (methods) together. The data (state) of a module is held by its methods via closure.
        - Singleton version  
            `// singleton version  
            var workshop = (function Module(teacher) {  
            var public = { ask };  
            return public;  
            function ask(question) {  
            console.log(teacher, question);  
            }  
            })("Eileen"); workshop.ask("is it a workshop?"); // "Eileen is it a workshop?"`