# Introduction
## Core features
[[Iteratee-First, Data-Last]]
[[Currying]]

### Currying




- 
- Every time a function is invoked, a unique context is created. If that function returns another function with references to variables in the parent function, each invocation of the parent function will result in a child function that references _different instances_ of those variables.  
```js
const parentFunction = function (msg) {  
let count = 0;  
function logger () {  
console.log(msg + ++count);  
}  

return logger;  
}  

const childInstance1 = parentFunction("Invocation #");  
const childInstance2 = parentFunction("Another invoke #");  

console.log(childInstance1()); // 'Invocation #1'  
console.log(childInstance1()); // 'Invocation #2'  
console.log(childInstance1()); // 'Invocation #3'  

console.log(childInstance2()); // 'Another invoke #1'  
console.log(childInstance2()); // 'Another invoke #2'  
console.log(childInstance2()); // 'Another invoke #3'  

// childInstance1 and childInstance2 retain references to  
// different instances of the count variable
```

## Function Purity

#### Functions vs. Procedures

##### What is a "procedure"?
- a collection of operations (things that you need to do in a program)
- just because it uses the `function` keyword does not make it a "function"

##### What is a "function"
- Takes some input **and** returns some output(s)
- **no** input is also a valid input, and `undefined` is a valid output as long as there is a semantic relationship to the input
            - There is an obvious semantic relationship between the input and the computed output
                - The name of the function should describe this relationship
                - Example  
                    `function shippingRate (size, weight, speed) {  
                    return ((size + 1) + weight) + speed;  
                    }`
                    - The name "shippingRate" describes the relationship between the function's inputs and its output
            - Minimizes **side effects**
                - Indirect inputs, or indirect outputs
                    - Using or changing parts of the program outside of the function
                - Some side effects are unavoidable
                    - I / O (console, files, etc.)
                    - Database Storage
                    - Network Calls
                    - DOM
                    - Timestamps
                    - Random Numbers
                    - CPU Heat
                    - CPU Time Delay
                - A program truly devoid of side effects is practically the same as the absence of that program
                    - You couldn't even prove that it existed
                - Avoid side effects when possible, make them obvious when necessary
                - Have a clear delineation between code that has side effects and code that is purely functional
                    - This makes code easier to debug because it's vastly more likely that bugs will crop up in code with side effects rather than in purely functional code
                - The problem with side effects is that the reader of your code has to read and mentally execute the entire program to be sure that a function will return the value that the reader expects
                    - To understand any given line, you have to read every line before that line to understand what state the program was in before you ran it
    - Pure functions & constants
        - A pure function:
            - Takes all of its inputs directly
            - Takes all of its outputs directly
            - Has no side effects
                - Does not rely on any specific outside value before the call
                - No outside value is changed after the call
            - Only closes over values that do not change (constants)
            - Given the same input will **always** return the same output
            - Has "referential transparency"
                - You could replace the function call with the return value of that function call without affecting any other part of the program
        - Constants
            - Constants are a semantic placeholder for a value
            - Constants do not change throughout the life of the program
            - If you reference an identifier that will never change, it is exactly the same as if the value had been written inline
            - Whether you use the `const` keyword or not, if you make it obvious to the reader that it will behave as a constant, it is okay to use in a pure function
    - Reducing surface area
        - If you reduce the number of lines that could theoretically reassign an outside value you reference in a function, you are reducing the amount of lines you have to understand to know whether or not the value ever got changed
        - Examples
            - Large surface area:  
                `var z = 1;  
                  
                function addTwo (x, y) {  
                return x + y;  
                }  
                  
                function addAnother (x, y) {  
                return addTwo(x, y) + z;  
                }  
                  
                addAnother(20, 21); // 42`
                - 11 lines in which Z could have been reassigned before `addAnother()` is called
            - Small surface area:  
                `function addAnother (z) {  
                return function addTwo (x, y) {  
                return x + y + z;  
                }  
                }  
                  
                addAnother(1)(20, 21); // 42`
                - Only two lines where Z would have been reassigned before `addAnother()` is called (i.e. lines 1.5 and 2.5)
    - Extracting impurity
        - If you have a procedure that is doing some pure computations and also causing some side effects, you can have them exist as two separate entities instead of one
        - Examples
            - Impure Procedure  
                `function addComment (userId, comment) {  
                var record = {  
                id: uniqueId(),  
                userId: userId,  
                text: comment  
                };  
                var elem = buildCommentElement(record);  
                commentsList.appendChild(elem);  
                }  
                addComment(42, "This is my comment!");`
            - Pure function, used in combination with impure side effects  
                `function newComment (userId, commentId, comment) {  
                var record = {  
                id: comentId,  
                userId: userId,  
                text: comment  
                };  
                return buildCommentElement(record);  
                }  
                  
                var commentId = uniqueId();  
                  
                var elem = newComment(  
                42,  
                commentId,  
                "This is my comment!"  
                );  
                  
                commentsList.appendChild(elem);`
        - Wherever possible, look for opportunities to extract out impurities, leaving a pure function behind
    - Containing Impurity
        - Wrapping
            - One way of containing impurities and preventing them from affecting large parts of the application is to **wrap them in a function**
            - Examples:
                - Unwrapped:  
                    `var SomeAPI = {  
                    threshold: 13,  
                    isBelowThreshold (num) {  
                    return num <= SomeAPI.threshold;  
                    }  
                    };  
                      
                    var numbers = [];  
                      
                    function insertSortedDesc (val) {  
                    SomeAPI.threshold = val;  
                    var index = numbers.findIndex(SomeAPI.isBelowThreshold);  
                    if (index == -1) {  
                    index = numbers.length;  
                    }  
                    numbers.slice(index, 0, val);  
                    }`
                    - modifying global numbers array, thus allowing side effect from `insertSortedDesc` function to leak out into the global scope
                - Wrapped:  
                    `var SomeAPI = {  
                    threshold: 13,  
                    isBelowThreshold (num) {  
                    return num <= SomeAPI.threshold;  
                    }  
                    };  
                      
                    var numbers = [];  
                      
                    function getSortedNums (nums, v) {  
                    var numbers = [...nums];  
                    insertSortedDesc(v);  
                    return numbers;  
                      
                    function insertSortedDesc (val) {  
                    SomeAPI.threshold = val;  
                    var index = numbers.findIndex(SomeAPI.isBelowThreshold);  
                    if (index == -1) {  
                    index = numbers.length;  
                    }  
                    numbers.slice(index, 0, val);  
                    }  
                    }`
                    - Now side effect from insertSortedDesc only modifies the local numbers array, preventing the side effect from affecting the global scope
            - Unfortunately wrapping isn't always possible because you may be modifying a value that you don't own and therefore cannot wrap in a function
        - Adapter Function
            - If a side effect is going to modify some outside value, and you can't just wrap the side effect to prevent it from doing so
            - How to use an argument adapter
                1. Capture the original state
                2. Set up appropriate initial state for the side effect
                3. Call the side effect
                4. Capture the modified state
                5. Restore the initial state
                6. Return the modified state
            - Write an adapter function alongside the function which contains the side effect
            - Brute force and ugly, not very scalable, but better than nothing.
            - Example:
                - Impure procedure with sibling adapter function  
                    `var SomeAPI = {  
                    threshold: 13,  
                    isBelowThreshold (num) {  
                    return num <= SomeAPI.threshold;  
                    }  
                    };  
                      
                    var numbers = [];  
                      
                    function insertSortedDesc (val) {  
                    SomeAPI.threshold = val;  
                    var index = numbers.findIndex(SomeAPI.isBelowThreshold);  
                    if (index == -1) {  
                    index = numbers.length;  
                    }  
                    numbers.slice(index, 0, val);  
                    }  
                      
                    function getSortedNums (nums, v) {  
                    var [oNumbers, oThreshold] = [numbers, SomeAPI.threshold]; // store original values  
                    numbers = [...nums];  
                    insertSortedDesc(v); // modifies global numbers var  
                    nums = numbers;  
                    [numbers, SomeAPI.threshold] = [oNumbers, oThreshold]; // reset global vars  
                    return nums; // return modified value  
                    }`  
                    
- Can only call other functions.
    - If it calls a procedure, it becomes a procedure, not a function.
- A function can return _multiple_ outputs

## Argument Adapters
    - a **unary** function takes _one_ input and returns _one_ output
    - a **binary** function is a function that takes two inputs and returns one output
    - **n-ary** functions take _n_ number of arguments
    - a **single-order function** is a function that neither takes functions as inputs nor returns functions as outputs
    - a **variadic function** is a function of indefinite arity (i.e., one which accepts a variable number of arguments.
    - An **argument shape adapter** is a type of _higher-order function_ which accepts a function as an argument, and modifies its shape (i.e. the number and kind of things that go into and out of it) by wrapping another function around it.
        - Example, adapt a function to take a specific # of arguments  
            `function binary (fn) {  
            return function two (arg1, arg2) {  
            return fn(arg1, arg2);  
            }  
            }  
              
            function f (...args) {  
            console.log(args);  
            }  
              
            binary(f)(1, 2, 3, 4); // [1, 2]`
    - Examples of adapters
        - flip  
            `function flip (fn) {  
            return function flipped (arg1, arg2, ...args) {  
            fn(arg2, arg1, ...args);  
            }  
            }  
            flip(logArgs)(1, 2, 3, 4); // [2, 1, 3, 4];  
            `
        - reverse  
            `function reverse (fn) {  
            return function reversed (...args) {  
            fn(...args.reverse());  
            };  
            }  
            reverse(logArgs)(1, 2, 3, 4); // [4, 3, 2, 1]`
## Point Free
    - **fixed point** functions
        - a _[fixed point](https://en.wikipedia.org/wiki/Fixed_point_(mathematics))_ of a function is a value that is mapped to itself by the function.
        - a **fixed point** (sometimes shortened to **fixpoint**, also known as an **invariant point**) of a [function](https://en.wikipedia.org/wiki/Function_(mathematics)) is an element of the function's [domain](https://en.wikipedia.org/wiki/Domain_of_a_function) that is mapped to itself by the function.
        - A point is a fixed point of a function if `f(x) = x`
    - **point free** functions
        - A class of techniques for defining functions by way of other functions
        - Point-free style is a way of defining functions with a very simple constraint: you cannot name arguments or intermediate values
        - Tacit programming, or programming in the “point-free” style, allows you to define a function without reference to one or more of its arguments
        - **Tacit programming**, also called **point-free style**, is a [programming paradigm](https://en.wikipedia.org/wiki/Programming_paradigm) in which function definitions do not identify the [arguments](https://en.wikipedia.org/wiki/Parameter_(computer_science)) (or "points") on which they operate.
            - Instead the definitions merely [compose](https://en.wikipedia.org/wiki/Function_composition_(computer_science)) other functions, among which are [combinators](https://en.wikipedia.org/wiki/Combinatory_logic) that manipulate the arguments
        - The lack of argument naming gives point-free style a reputation of being unnecessarily obscure, hence the epithet "pointless style".
    - equational reasoning
        - reasoning about whether one expression is equal to (i.e. equivalent to or interchangable with) another
    - Composition
        - When a function's output becomes the input to another function

## Closure
    - Closure is not necessarily pure because a value that a function closes over may be changed by that function
        - Thus, closure is only pure if the value closed over is a constant
    - If you're using closure in functional programming, it is important that you do not mutate the values that you're closing over
    - Lazy vs. Eager Execution
        - Eager execution is when you execute a computation immediately
        - Lazy execution is when you defer the work to a function, so that the work only actually gets done if that functiono gets called.
        - Pros & Cons
            - Pro: If you're not sure that under all conditions the function will be called, you're not wasting time doing unnecessary work
            - Con: If the function will get called a lot, you're doing the work every time that function gets called
        - Example  
            `function eagerRepeater (count) {  
            var str = ("").padStart(count, "A");  
            return function allTheAs() {  
            return str;  
            }  
            }  
              
            function lazyRepeater (count) {  
            return function allTheAs() {  
            return ("").padStart(count, "A");  
            }  
            }`
    - Memoization
        - an optimization technique used primarily to speed up computer programs by storing the results of expensive function calls and returning the cached result when the same inputs occur again
        - Use cases:
            - If you expect a function to be called multiple times with the same inputs
        - Example memoize utility  
            `function memo(func){  
            var cache = {};  
            return function(){  
            var key = JSON.stringify(arguments);  
            if (cache[key]){  
            return cache[key];  
            } else{  
            val = func.apply(null, arguments);  
            cache[key] = val;  
            return val;  
            }  
            }  
            }`
    - Referential transparency
        - Useful for readers of code because once they have mentally computed the return value of a function once, they can replace all future calls to that function mentally with its return value instead of having to mentally compute the value again
    - Generalized to Specialized
        - One of the reasons why you make something a function is so that its name is more semantic
            - Less semantic:  
                `ajax(CUSTOMER_API, 42, renderCustomer)`
            - More semantic:  
                `getCustomer(id, cb) {  
                return ajax(CUSTOMER_API, id, cb);  
                }`
            - Even more semantic:  
                `getCurrentUser(cb) {  
                return getCustomer(auth.id, cb);  
                }`
            - By making `getCurrentUser` call `getCustomer` instead of `ajax`, it is more clear that `getCurrentUser` is the **specialization** of `getCustomer`, which is a stronger statement than saying it is the specialization of `ajax`
        - When specializing a function, its parameters should be supplied in order of **most general -> most specific**
            - This is because you will provide the parameters one at a time, and you want to unfold them in that order
            - Example:
                - `function map (fn, array) { ... }`
                - a callback function is more general a function than is an array of data
                - data is the MOST specific input that can go in, so it should be last
    - Partial Application & Currying
        - Partial Application
            - Most functional libraries will provide some sort of implementation of `partial`
                - it takes a function as its first input, and a list of arguments to pass to that function at some point
                - it returns a function which calls the function with the arguments provided
                - It returns a function which accepts any arguments which were not provided to the original function via the call to `partial`
                - This function is a **partial application** of the original function
                - Example:  
                    `function ajax (url, data, cb) { /* ... */ }  
                      
                    var getCustomer = partial(ajax, CUSTOMER_API);  
                    // getCustomer is a partial application of ajax  
                      
                    var getCurrentUser = partial(getCustomer, { id: 42 });  
                    // getCurrentUser is a partial application of getCustomer`
            - Partial application allows you to preset one or more of the inputs a function accepts, and produce a function which expects the rest of the inputs
            - "partially applying some of the inputs now, supplying the rest later"
        - Currying
            - Much more common form of specialization
            - Most functional libraries provide a utility called `curry`
                - `function curry (numberOfArgs, fn) { /* ... */ }`
                - First argument is the number of arguments the curried function will take (i.e. how many intermediary functions could be created)
                - Second argument is the final function to be called once all the arguments have been provided
                - Returns a new function every time it's called until all arguments have been provided
            - A curried function is essentially a chained series of unary functions
            - strict vs. loose currying
                - strict currying means you can only pass in one argument at a time  
                    `logXYZ(1)(2)(3); // strict currying`
                - loose currying means you can pass one **or more** arguments at a time  
                    `logXYZ(1, 2)(3); // loose currying`
        - Currying vs. Partial Application
            - Currying is usually preferred because its more convenient to only have to call the utility once
            - Partial Application might be better, however, if you have a function that accepts 5 arguments, and you want to produce a function that accepts 3 arguments, for example
                - Currying might not work for 2 reasons:
                    - You'd have to call the result of `curry` twice to produce the desired function
                    - The resulting function would not be a single function expecting 3 arguments, rather it would be a series of unary functions which would need to be called one at a time (in strict currying)
## Composition
    - Abstration
        - **abstraction** means taking two things which are intertwined in a program, separating them, and inserting a semantic boundary between them
            - The goal being that you can look at either one of the things and understand it, without needing to understand the other
    - Pipe vs. Compose
        - `compose` utilities typically take their arguments right to left
        - `pipe` utilities typically take their arguments left to right
    - Composition is associative
        - you can compose functions in any grouping, but as long as they're composed in the **same order**, the resulting functions will produce the same outputs
        - Example  
            `var f = compose(compose(minusTwo, triple), increment);  
            var p = compose(minusTwo, compose(triple, increment);  
              
            f(4) == p(4); // true`
        - This means that you can curry or partially apply the compose utility
    - Currying + Composition
        - Since functions generally only produce one output, it's vastly more common & convenient to compose unary functions
        - You can use currying to transform binary functions into unary functions, which allows them to be composed
        - Example  
            `function sum (y, x) { return x + y }  
            function triple (x) { return x * 3 }  
            function divideBy (y, x) { return x / y }  
              
            sum = curry(2, sum);  
            divideBy = curry(2, divideBy);  
              
            compose(  
            divideBy(2), // provide divideBy its 1st argument, returns unary function  
            triple,  
            sum(3) // provide sum its 1st arg, returns unary function  
            )(5); // 12`
## Immutability
    - There are 2 kinds of immutability
        - **Assigment Immutability**
            - When a value is assigned to a variable, that variable is no longer able to be reassigned to another value.
        - **Value Immutability**
            - When the value that is assigned to a variable cannot be changed without reassigning the variable.
    - Some value types are mutable (objects), some are not (primitives)
        - You can't take the string `"Hello world"` and change it into something else, you can only return a NEW string
    - `const`
        - only use `const` for declaring **constants**
            - a constant is a semantic placeholder for a value
    - Read-only data structures
        - Read only data structures and immutable data structures are _not_ the same thing
        - When passing an object to a function, and it is important that that object not be mutated, always pass it with `Object.freeze()`
            - _The Object.freeze method freezes an object: that is, prevents new properties from being added to it; prevents existing properties from being removed; and prevents existing properties, or their enumerability, configurability, or writability, from being changed._
            - `Object.freeze()` sets an object's (shallow) properties to read only, and therefore cannot be reassigned
        - When writing a function that accepts a mutable data structure as a parameter, **always** assume that the parameter is read-only
            - By mutating the object passed in, you are creating side effects which could potentially introduce bugs in other parts of the program
    - Immutable Data Structures
        - An immutable data structure allows only **structured** mutation (as opposed to simple assignment)
            - Can only be mutated via an API
            - Rather than mutating the original data structure, by default the API will return you a **new** data structure, with the mutatino having been applied to it.
        - Immutable data structures are designed in such a way as to mitigate the performance overhead of having to create a new object every time the original object needs to be mutated
            - They do this by, rather than creating a copy of the entire object just to reflect the change, they essentially store a "diff" (like a git repo)
            - This means that the object contains the changed property, as well as a pointer back to the original for all the rest
        - Unfortunately, JavaScript does not have Immutable Data Structures natively
            - There are libraries that provide IDS's, like [Immutable.js](facebook.github.io/immutable-js)
    - A shorthand for returning 1, -1, or 0 in a sort comparator function is just to take the first value minus the second value
        - `arr.sort(function asc (x, y) { return x - y; });`
            - if x is less than y, it returns a negative number
            - if x is greater than y, it returns a positive number
            - if x is equal to y, it returns zero
## Recursion
    - Reduce the problem set
    - Every recursive algorithm has, at a minimum, a **base condition** (i.e. how you know when to stop)
    - Stack Frames & Memory Limits
        - Every time a function gets called, an area of memory for the information produced in that function is created, which is called a **stack frame** or **memory frame**
    - Tail Calls
        - Tail calls are function calls which occur at the **end** (the _tail_) of the execution logic
        - The benefit of tail calls is that at any given time you only need a single stack frame
            - by the time a tail call is made, there is no more work to be done in the parent stack frame, so it can be removed
    - PTC (Proper Tail Calls)
        - The idea that a tail call gets memory optimized
        - TCO (Tail Call Optimization)
            - A family of optimizations for tail calls (which includes PTC) which engines may or may not implement
        - Standardization
            - You must be in strict mode
            - Function call must be in "proper tail position"
                - Nothing must happen after the function call except that its value is returned
                - It must have a `return` keyword
                - The function call **may** be in one of the branches of a ternary statement or an `if` block (because the condition is computed before the value is returned)
        - To figure out if a recursive algorithm can be modified to take advantage of tail calls:
            - Is there any information in the current stack frame that needs to be kept track of while the next function is executing?
        - Currently, only Safari supports PTC
    - Continuation-Passing style
    - Trampolines
        - Instead of having a function that calls a function that calls a function, you have a function that **returns** a function which makes the next call
## List Operations
    - Functors
        - A **functor** is a value containing other values which can be _mapped_ over.
    - **Map**: Transformation
        - Map is a transformation operation in which you perform a transformation on each of the values in a functor and project them into a **new** data structure
        - Map is a pure function which does not mutate the functor
        - Map always results in the same kind of data structure that it started with
        - Map performs the same tranformation on all the values in the source data structure
    - **Filter**: Inclusion
        - Filter is an **inclusive** operation, meaning that rather than determining what values get _removed_ from the original data structure, it determines what values get _included_ in the new data structure.
    - **Reduce**: Combination
        - Reduce takes an initial value
        - Reduce takes two values from a data structure, whether the initial value and the first value, or two consecutive values, and "reduces" them to a single value using a provided reducer function
        - Unlike Map, Reduce does not have to produce the same kind of data structure as the data structure being reduced
        - Basically any other list operation can be replicated using reduce, it is very versitile
    - Transduction
        - Transduction is useful for when you have a Map, Reduce, and/or Filter that you need to compose together
        - A transduction is a mathematical transformation of 2 or more functions so that they take on shapes which can be composed together
            - Composition of reducers
            - You take **projector** (map) functions and **predicate** (filter) functions, and transform them into **combinator** (reducer) functions which can be composed together
        - Functional libraries will typically offer utilities like `mapReducer` and `filterReducer` which are modified versions of `map` and `reduce` which can be composed together
            - The result of composing `mapReducer` and `filterReducer` is a kind of proto-reducer which still needs a reducer to be passed to them in order to become a reducer
                - This "proto-reducer" is called a **transducer**
                - A transducer is a type of **higher-order reducer**
        - You pass the transducer to a utility usually called `transduce`, along with a combinator (reduce function), your initial value, and the functor (data structure to be reduced)
        - Functional libraries often provide a utility called `into` which does the exact same thing as `transduce`, except allows you to skip supplying a custom combinator and instead assumes what combinator you want based on the data type of the functor (i.e. addition for numbers, concatenation for strings, `.push` for arrays, etc.)
        - Deriving Transduction

## Data Structure Operations

### Monad Data Structures
- A monad is a way of creating a functional-friendly data structure
- A wrapper around a value with a set of behaviors, which makes it easier to interoperate with other values in a specific way
- Turns a single value into a functor
- A pattern for pairing data with a set of predictable behaviors that let it interact with other data + behavior pairings (monads)

#### Types of Monads:
- "Just" monad
- "Maybe" monad
- "Nothing" monad

