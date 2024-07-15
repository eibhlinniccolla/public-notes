- What is `this`?
    - Common rule-of-thumb
        - `this` refers to whatever is to the left of the dot of a method invocation
    - value is either
        - reference to an object (non-strict mode)
        - any value (strict mode)
    - Basically JavaScript's version of dynamic scope
        - Lexical Scope:
            - The identifiers that a function has access to are determined at _compile time_ by where that function is _defined_
        - Dynamic Scope
            - The identifiers that a function has access to are determined at _run time_ by the execution context in which the function is _invoked_
        - This is only half-true though, because dynamic scope in JavaScript works very differently from how it works in a language like Perl
            - Perl example:  
                ```$var = "Global";  
                  
                sub inner {  
                print "inner: $var\n";  
                }  
                  
                sub changelocal {  
                my $var = "Local";  
                print "changelocal: $var\n";  
                  
                &inner  
                }  
                  
                sub changedynamic {  
                local $var = "Dynamic";  
                print "changedynamic: $var\n";  
                  
                &inner  
                }  
                  
                &inner  
                &changelocal  
                &changedynamic```
            - JS example:  
                ```var name = "Eileen";  
                  
                function inner () {  
                console.log(`inner: ${this.name}`);  
                }  
                  
                function changeLocal () {  
                var name = "Jessie";  
                var obj = { name };  
                console.log(`changeLocal: ${obj.name}`)  
                  
                inner.call(obj);  
                }  
                  
                function changeDynamic () {  
                var name = "Tyler";  
                console.log(`changeDynamic: ${name}`)  
                  
                inner();  
                }  
                  
                inner();  
                changeLocal();  
                changeDynamic();  
                  
                /*  
                inner: Eileen  
                changeLocal: Jessie  
                inner: Jessie  
                changeDynamic: Tyler  
                inner: Eileen  
                */```
    - common misconceptions
        - `this` refers to the function itself (Incorrect)
        - `this` refers to the function's scope (Incorrect)
    - property of an execution context (global, function or eval)
        - When a function is invoked, an activation record (aka execution context) is created
        - execution context contains information about
            - where the function was called from (the call-stack)
            - _how_ the function was invoked
            - what parameters were passed
            - the `this` reference for the function's execution
- How is the value of `this` determined?
    - (usually) determined by how a function is called (runtime binding)
    - Finding the call site
        - Using developer tools to find call-site
            - use the developer tools to get the call-stack
            - find the second item from the top
            - call-site is _in_ the invocation _before_ the currently executing function
- What is `this` used for?
    - `this` allows an object method to access the information stored on that object
    - A `this`-aware function can thus have a different context each time it is called, which makes it more flexible & reusable
    - the this mechanism provides a more elegant way of implicitly "passing along" an object reference
- There are 4 different ways to call a function, each providing a different binding to `this`
    - implicit binding
        - When you call a this-aware function as a method on an object
        - This is not affected by how or where a function is **defined**, only how it was **called**
        - only affected by the most immediate member reference
            - Example  
                `o.b = {  
                g: function() {  
                return this.p  
                },  
                p: 42  
                };  
                o.b.g(); // 42`
        - If the method is on an object's prototype chain, `this` refers to the object the method was called on, as if the method were on the object.
            - Example  
                `var o = {f: function() { return this.a + this.b; }};  
                var p = Object.create(o);  
                p.a = 1;  
                p.b = 4;  
                  
                console.log(p.f()); // 5`
        - A function used as getter or setter has its `this` bound to the object from which the property is being set or gotten.
            - Example:  
                `function sum() {  
                return this.a + this.b + this.c;  
                }  
                  
                var o = {  
                a: 1,  
                b: 2,  
                c: 3,  
                get average() {  
                return (this.a + this.b + this.c) / 3;  
                }  
                };  
                  
                Object.defineProperty(o, 'sum', {  
                get: sum, enumerable: true, configurable: true});  
                  
                console.log(o.average, o.sum); // 2, 6`
        - Example  
            `var person = {  
            name: 'Eileen',  
            sayHello: function() {  
            console.log('Hi, my name is' + this.name);  
            }  
            }  
              
            person.sayHello(); // 'Hi, my name is Eileen'`
    - explicit binding
        - when a function is explicitly given a value to use as its `this`
            - with `.call()`, `apply()`, or `.bind()`
        - `call` vs. `apply`
            - functions are actually objects
                - when you call a function using parentheses, you're actually calling its `.call()` method
            - both accept `this` object as first parameter
            - `call` takes a list of arguments, `apply` takes an array
            - In non-strict mode, if the supplied `this` object is not an object, the abstract operation `ToObject` will be applied
        - `bind`
            - takes `this` object as first parameter
            - returns a NEW function with the `this` value set to the supplied object
            - does NOT call the function, unlike `.call()` and `.apply()`
            - Using `.bind()` should be the exception, not the rule.
                - If you find yourself using it a lot, you probably should just use a closure
            - As of ES6, the hard-bound function produced by bind(..) has a .name property that derives from the original _target function_. which is the function call name that should show up in a stack trace.
            - Many libraries' functions, and indeed many new built-in functions in the JavaScript language and host environment, provide an optional parameter, usually called "context", which is designed as a work-around for you not having to use bind(..) to ensure your callback function uses a particular this.
        - If you pass a simple primitive value (of type string, boolean, or number) as the this binding, the primitive value is wrapped in its object-form (new String(..), new Boolean(..), or new Number(..), respectively). This is often referred to as "boxing".
    - the `new` keyword
        - Invokes the function with the `this` keyword pointing at an empty object
        - JavaScript has a new operator, and the code pattern to use it looks basically identical to what we see in those class-oriented languages; most developers assume that JavaScript's mechanism is doing something similar. However, there really is _no connection_ to class-oriented functionality implied by new usage in JS.
        - First, let's re-define what a "constructor" in JavaScript is. In JS, constructors are just functions that happen to be called with the new operator in front of them. They are not attached to classes, nor are they instantiating a class. They are not even special types of functions. They're just regular functions that are, in essence, hijacked by the use of new in their invocation.
        - This is an important but subtle distinction: there's really no such thing as "constructor functions", but rather construction calls _of_ functions.
        - When a function is invoked with new in front of it, otherwise known as a constructor call, the following things are done automatically:
            - a brand new object is created (aka, constructed) out of thin air
            - _the newly constructed object is [[Prototype]]-linked_
            - the newly constructed object is set as the this binding for that function call
            - unless the function returns its own alternate object, the new-invoked function call will _automatically_ return the newly constructed object.
        - Why is new being able to override _hard binding_ useful?
        - The primary reason for this behavior is to create a function (that can be used with new for constructing objects) that essentially ignores the this _hard binding_ but which presets some or all of the function's arguments. One of the capabilities of bind(..) is that any arguments passed after the first this binding argument are defaulted as standard arguments to the underlying function (technically called "partial application", which is a subset of "currying").
    - default binding
        - In sloppy mode
            - (Browser) `this` defaults to the `window` object
            - (Node) `this` defaults to the `global` object
        - In strict mode
            - `this` defaults to `undefined`
- Binding precedence
    1. `new` binding
    2. explicit binding
    3. implicit binding
    4. default binding
- Losing `this`
    - Common issues:
        - `this`-aware function passed as callback _loses_ this value
        - callback function _reassigns_ `this` value for the call
            - common example is event handlers in JS libraries change `this` to point at DOM element
    - Be aware of what callsite for function looks like
        - Example  
            `setTimeout(eileen.sayHello, 1000)`  
              
            When the function is eventually gets invoked, it will probably look like this:  
              
            **`{ ... cb(args) ... }`**
        - You can't rely on implicit binding, since you're only passing a reference to a function, not its `this`
    - Reference Type
        - a “specification type”
            - We can’t explicitly use it, but it is used internally by the language.
        - The value of Reference Type is a three-value combination `(base, name, strict)`, where:
            - `base` is the object.
            - `name` is the property name.
            - `strict` is true if use strict is in effect.
        - The result of a property access is not a function, but a value of type `ReferenceType`
            - For user.hi in strict mode it is:  
                `// Reference Type value  
                (user, "hi", true)`
        - When parentheses () are called on the Reference Type, they receive the full information about the object and its method, and can set the right `this`
        - Reference type is a special “intermediary” internal type, with the purpose to pass information from dot . to calling parentheses ().
        - Any other operation like assignment hi = user.hi discards the reference type as a whole, takes the value of user.hi (a function) and passes it on. So any further operation “loses” this.
- Ignored `this`
    - If you pass null or undefined as a `this` binding parameter to call, apply, or bind, those values are effectively ignored, and instead the _default binding_ rule applies to the invocation.
    - It's quite common to use apply(..) for spreading out arrays of values as parameters to a function call. Similarly, bind(..) can curry parameters (pre-set values), which can be very helpful.
    - However, there's a slight hidden "danger" in always using null when you don't care about the this binding. If you ever use that against a function call (for instance, a third-party library function that you don't control), and that function _does_ make a this reference, the _default binding_ rule means it might inadvertently reference (or worse, mutate!) the global object (window in the browser).
    - Perhaps a somewhat "safer" practice is to pass a specifically set up object for this which is guaranteed not to be an object that can create problematic side effects in your program.
        - If we always pass a DMZ object for ignored this bindings we don't think we need to care about, we're sure any hidden/unexpected usage of this will be restricted to the empty object, which insulates our program's global object from side-effects.
    - Indirection
        - One of the most common ways that _indirect references_ occur is from an assignment:  
            ```function foo() {  
            console.log( this.a );  
            }  
              
            var a = 2;  
            var o = { a: 3, foo: foo };  
            var p = { a: 4 };  
              
            o.foo(); // 3  
            (p.foo = o.foo)(); // 2```
        - The _result value_ of the assignment expression p.foo = o.foo is a reference to just the underlying function object. As such, the effective call-site is just foo(), not p.foo() or o.foo() as you might expect. Per the rules above, the _default binding_ rule applies.
    - Softening Binding
        - The problem is, _hard-binding_ greatly reduces the flexibility of a function, preventing manual this override with either the _implicit binding_ or even subsequent _explicit binding_ attempts.
        - It would be nice if there was a way to provide a different default for _default binding_ (not global or undefined), while still leaving the function able to be manually this bound via _implicit binding_ or _explicit binding_ techniques.
        - We can construct a so-called _soft binding_ utility which emulates our desired behavior.  
            ```if (!Function.prototype.softBind) {  
            Function.prototype.softBind = function(obj) {  
            var fn = this,  
            curried = [].slice.call( arguments, 1 ),  
            bound = function bound() {  
            return fn.apply(  
            (!this ||  
            (typeof window !== "undefined" &&  
            this === window) ||  
            (typeof global !== "undefined" &&  
            this === global)  
            ) ? obj : this,  
            curried.concat.apply( curried, arguments )  
            );  
            };  
            bound.prototype = Object.create( fn.prototype );  
            return bound;  
            };  
            }```
        - The softBind(..) utility provided here works similarly to the built-in ES5 bind(..) utility, except with our _soft binding_ behavior. It wraps the specified function in logic that checks the this at call-time and if it's global or undefined, uses a pre-specified alternate _default_ (obj). Otherwise the this is left untouched. It also provides optional currying (see the bind(..) discussion earlier).
- Lexical `this`
    - Arrow functions do not define a **`this`** value at all.
    - Because of this, arrow functions cannot be called with `new`
    - **`this`** it will behave just as any other variable
    - JavaScript will look into the enclosing **lexical** scope for a defined **`this`** keyword
    - Only use arrow functions when you need lexical `this`
    - If a `this` argument is passed to the `.bind`, `.call`, or `.apply` method of an **arrow function**, it will be **ignored**
    - While arrow-functions provide an alternative to using bind(..) on a function to ensure its this, which can seem attractive, it's important to note that they essentially are disabling the traditional this mechanism in favor of more widely-understood lexical scoping.
    - Example  
        `var person = {  
        name: 'Eileen',  
        sayHello: function () {  
        setTimeout(() => {  
        /* no this is defined, so it looks to the enclosing lexical scope, which is the function sayHello */  
        console.log("Hi " + this.name + "!");  
        }  
        }  
        }  
          
        person.sayHello();  
        // "Hi Eileen!"`