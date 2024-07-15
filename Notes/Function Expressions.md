- A "function expression" is a function that is assigned as a value
    - In JS, functions are "first class objects", i.e. objects that can be passed around as values
    - function expressions can either be **anonymous function expressions** or **named function expressions**
    - named function expressions are preferrable where possible, because descriptive names tell us what the function is doing.
- _Named_ vs. _Anonymous_ Function Expressions
    - tldr;, named function expressions have names, anonymous function references do not.  
	```js 
// named function expression  
var foo = function myFn () {};  
  
// anonymous function expression  
var bar = function () {}  
  
// named function expression  
baz(function myOtherFn () {});  
  
// anonymous function expression  
baz(function () {});```
    - a **named** function expression is a function expression that is assigned to an identifier within its **own** scope
        - (i.e. the identifier can be referenced from within the function itself, but not outside it)  
            ```js
            var foo = function bar () {  
            console.log(bar);  
            }  
              
            foo(); // [Function bar]  
            bar(); // ReferenceError: bar is not defined  
            ```
        - This reference is also **read-only**  
            ```js
            var foo = function bar () {  
	            bar = 12;  
            }  
              
            foo(); // TypeError: Assignment to constant variable.  
            ```
        - This contrasts with function **declarations** in that function declarations have their identifiers added to the **enclosing** scope, whereas named function **expressions** have their identifiers added to **their own** scope  
```js
function first() {  
	console.log(first)  
}  
  
var secondVar = function second () {  
	console.log(second);  
}  
  
first(); // [Function first]  
secondVar(); // [Function second]  
second(); // ReferenceError
```
- an **anonymous** function expression, however, has no internal self reference.
	- It can reference the identifier it was assigned to if it is part of an assignment expression, but otherwise it has no way of referencing itself.
	- This external self-reference is **not** read only, and can cause difficult to diagnose errors  
	```js
	var foo = function () {  
		console.log("this will only run once!");  
		foo = 42;  
	}  
	  
	foo(); // "this will only run once!"  
	foo(); // TypeError: foo is not a function  
	```
    - You should use _named_ function expressions 100% of the time
        - Produces a reliable, read-only self-reference (recursion, etc.)
            - It is less prone to error to reference an identifier within the current scope that is read only
        - More debuggable stack traces
        - Code is more self-documenting
    - If you can't think of a good name, name it TODO so you remember to come back and name it
- Function expression rules of thumb
    - If it's more than 3 lines of code, make it a function declaration
- IIFEs
    - IIFEs are useful because they create a new block of scope. This encapsulates behavior inside the function to prevent them from leaking out and affecting global scope.
    - The same thing can be accomplished using block scope and `let`/`const`
    - However unlike block scopes, if you need a statement or scope in an expression position, you can wrap it in an IIFE
        - No IIFE  
	```js
	var teacher;  
	try {  
		teacher = fetchTeacher();  
	} catch (err) {  
		teacher = "Eileen";  
	}
	```
- IIFE  
	```js
	var teacher = (function getTeacher () {  
		try {  
			return fetchTeacher();  
		} catch (err) {  
			return "Eileen";  
		}  
	})();
	```