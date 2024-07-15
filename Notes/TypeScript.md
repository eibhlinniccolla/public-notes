- Understanding TypeScript - Max Schwarzmüller
    - ~~Using Types~~
        - ~~Data types~~
            - [[Tuple]]
            - [[Enum]]
	[[Using types in functions (TypeScript)]]
	[[Functions as types (TypeScript)]]
        - ~~Objects and types~~
            - if you create an object with certain properties and values, TypeScript infers the types of those values using the property name and enforces them  
                let myObj = { name: "sophie", age: 24 };  
                myObj = {}; // this produces an error  
                myObj = { a: "bob", b: 55 }; // so does this
            - You can specify the format an object should follow the same way as you can a function  
                let myObj: { name: string, age: number };  
                myObj = { name: "sophie", age: 24 }; // this works
                - You can also immediately initialize the variable after its type declaration  
                    let newObj { prop: number } = {  
                    prop: 10  
                    }
[[Type Alias (TypeScript)]]
        - ~~Union types~~
            - Union types allow the possibility of more than one type to be specified to the exclusion of all others, unlike **any**  
                let ageOrName: number | string = 12;  
                ageOrName = "Sophie"; //This doesn't produce an error.
            - Use a pipe to separate the desired types
        - ~~Check types during runtime~~
            - when doing type comparisons using **typeof** you have to compare it against the type name in quotes  
                let myNum: number = 24;  
                if (typeof myNum == "number") // This code will run
        - ~~The "never" type~~
            - ~~used to declare the type of a function which never returns  
                function neverReturns (): never {  
                throw new Error("An error!");  
                }~~
            - note that this is not the same as type **void** because a void function returns, it just doesn't return anything. A **never** function _never_ returns at all.
        - ~~Nullable types~~
            - By default, a variable of any type, including **any** and **undefined** can be assigned the value null.
            - By setting "strictNullChecks" in the tsconfig.json file to **true**, you can override this behavior.
                - Now "null" is treated just like the other types.
                - A type non-null variable cannot be assigned the value null
                - A type null variable cannot be assigned a value pertaining to another type, such as string or number
                - A type **any** variable can still be assigned the value _null_
                - To allow a type non-null variable to accept the value _null_, it must be explicitly declared as being nullable  
                    let nullableVariable: number | null = 12;  
                    nullableVariable = null; // No problem
    - ~~TypeScript Compiler~~
        - How code gets compiled
            - By default, if there are type errors in your code, the TSC will still compile the code but gives you warnings in the console.
        - Changin compiler behavior on errors
            - "noEmitOnError" - prevents compilation on error
        - Debugging using SourceMap
            - "sourceMap" - causes .map file to be created when compiling
            - Allows you to debug directly in your TypeScript code by clicking sources, then file.ts
        - Avoiding Implicit Any
            - "noImplicitAny" - prevents TSC from assigning type **any** to variables which do not have a type declared.
        - Preventing Unused Parameters
            - "noUnusedLocals" - lets you know if any parameters are assigned but never used.
    - ~~ES6~~
        - ~~let & const~~
            - _let_ creates block scope variable
            - _var_ creates global scope variable
            - _const_ creates a constant
                - constants are immutable data structures whose values cannot change once assigned
        - ~~block scope~~
            - hoisting
                - var, let, and const declarations are all hoisted to the top of their scope
                    - var gets hoisted to the top of the global or function scope
                    - let and const get hoisted to the top of the block scope
                - hoisted var declarations are initialized with a value of undefined
                - hoisted let and const definitions are not initialized, and referencing them before the let statement is evaluated results in a reference error
                - const cannot be declared without also being initialized.
                - The TypeScript compiler will give you an error about this, however it will not result in a reference error.
                    - TypeScript will change all let and const declarations to var, and rename them where necessary.
                    - Accessing them before they are initialized will return undefined, not a reference error.
        - ~~arrow functions, lambda expressions, anonymous functions~~
            - ~~A lambda is a function that is used as data~~
                - The function is:
                    - passed as an argument to another function
                    - returned as a value from a function
                    - assigned to variables or data structures.
                - This is basically the definition of a **first class function**
                - In JavaScript, any function can be a lambda expression because **all functions are first-class**.
                - Not all lambda expressions are anonymous.
            - ~~An anonymous function is a function without a name~~
                - Not all anonymous functions are lambdas  
                    (message) => {  
                    console.log(message);  
                    }("my message")
                    - this function is just evaluated and then dropped on the floor
            - Arrow functions
                - Arrow functions are expressions that create anonymous functions  
                    var anon = (a, b) => { return a + b };
                    - If you only have one line, you can ditch the braces  
                        var anon = (a, b) => a + b;
                    - If you only have one parameter, you don't need the parentheses either  
                        var anon = a => a;
                        - However, if you leave out the parentheses, you cannot include a type declaration  
                            var anon = a: string => a;  
                            // This causes an error
                - Arrow functions do not have their own **this** value. **this** is bound to the enclosing scope.
                - When returning object literals, it's best to wrap them in parentheses  
                    ( () => ( {foo: 1} ) )
        - ~~default parameters~~
            - You can give a function's parameters a default value by assigning it in the function definition  
                function myFunction (myNum: number = 5) {  
                console.log(myNum);  
                }  
                  
                myFunction(); //This returns 5
            - You can also have one of the parameters be the result of an expression using another one of the parameters  
                function foo (first: number = 5, second: number = first - 2) {  
                console.log(second);  
                }  
                  
                foo(); // returns 3  
                foo(2) //returns 0  
                foo(4, 5) //returns 5
        - ~~spread & rest operators~~
            - spread
                - it takes an array and transforms it into a list of values.
                - is used in function calls  
                    const numbers = [1, 2, 3];  
                    console.log(...numbers);
            - rest
                - takes a list of values and transforms it into an array
                - is used in function definitions  
                    function makeArray (...numbers: number[]) {  
                    return numbers;  
                    }  
                    makeArray(1, 4, 3, 9); // [1, 4, 3, 9]
        - ~~destructuring~~
            - Reference
                - https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment
            - Destructuring allows you to unpack values from an array or object and store them in variables.  
                var foo = ['one', 'two', 'three'];  
                  
                var [one, two, three] = foo;  
                console.log(one); // "one"  
                console.log(two); // "two"  
                console.log(three); // "three"
            - Variables can be assigned their values via destructuring separate from their declaration.  
                var a, b;  
                [a, b] = [1, 2]  
                console.log(a) // 1  
                console.log(b) // 2
            - Arrays
                - to destructure an array, use square brackets on the left side of the equals sign  
                    const [x, y, z] = [1, 2, 3];
            - Objects
                - to destructure an object, use curly braces on the left side of the equals sign  
                    const {first, last} = {first: "sophie", last: "mccall"};
                - Order doesn't matter, but the property names must match.  
                    const obj = { first: "sophie", last: "mccall" };  
                    let { firstName, lastName } = obj // this doesn't work  
                    let { last, first } = obj; // but this does
                - Reassign names with aliases using a colon  
                    const object = { name: "sophie" };  
                    let { name: firstName } = object;  
                    console.log(firstName); // "sophie"
        - ~~template literals~~
            - template literals allow you to have multiline strings which contain variables  
                const myString = `this is a  
                multiline string,  
                how cool!`; // the line breaks here will be preserved
            - you can use variables within a template literal using ${}  
                let name = "sophie";  
                let myString = `Hi! My name is ${name}! How are you?`;
    - Using Classes
        - ~~Classes and Class Properties~~
            - you can create a class using the **class** keyword
            - properties can be created on classes directly, you don't need to use **this**  
                class Dog {  
                name: string;  
                }
            - When creating a property, you end it with a semicolon, not a comma  
                class Person {  
                name: string, // no bueno  
                age: number; // no problemo  
                }
            - properties can be made public using the **public** keyword  
                class Dog {  
                public name: string;  
                }
                - public properties are accessible from outside the class itself
                - properties are public by default
            - properties are made private using the **private** keyword  
                class Dog {  
                private weight: number;  
                }
                - private properties are only available within the class
            - properties are made protected using the **protected** keyword  
                class Dog {  
                protected breed: string;  
                }
                - protected properties are only available within the class itself, or within classes which inherit from that class
            - create the constructor function of a class using the **constructor** keyword
                - the constructor function is the function that gets called when you new up an instance of a class  
                    class Dog {  
                    name: string;  
                    constructor (name: string) {  
                    this.name = name;  
                    }  
                    }  
                      
                    let sparky = new Dog("Sparky");  
                    console.log(sparky.name); // "Sparky"
                - the constructor function allows you to set properties directly on the class using the public keyword  
                    class Cat {  
                    constructor (public name: string) {  
                    }  
                    }  
                    let mews = new Cat("Mews");  
                    console.log(mews.name); // "Mews"
            - properties are accessed within a class using the **this** keyword
        - ~~Class Methods and Access Modifiers~~
            - you can create a method on a class by simply defining it, you don't need the **function** keyword  
                class Dog {  
                woof () {  
                console.log("woof");  
                }  
                }
            - methods can be made private and protected just like other properties
            - private and protected variables are available to methods within a class, which means you can use those methods to modify private and protected properties  
                class Man {  
                private age: number = 28;  
                setAge(age: number) {  
                this.age = age;  
                console.log(this.age);  
                }  
                }  
                let pat = new Man();  
                pat.setAge(30); //sets pat.age equal to 30, even though it's private.
        - ~~Inheritance~~
            - you can use the **extends** keyword to create a class that implements all the same code as another class
        - ~~Constructors and Inheritance~~
            - When defining a constructor for a class that implements another class, you have to call _super()_ within the constructor.
            - Super() executes the constructor of the parent class.
            - You must somehow provide all of the arguments that the parent class's constructor expects.
        - ~~Getters & Setters~~
            - Getters and Setters are used to make sure that certain criteria are met, or to execute code before setting a value or returning a value.
            - getters
                - you create a getter by using the **get** keyword, followed by the property name with parens and then braces
            - setters
                - you create a setter by using the **set** keyword, followed by the property name with parens and then braces
        - ~~Static Properties & Methods~~
            - You can make a property or method of a class accessible without instantiating the class by making the method/property **static**
            - Make a property/method static by using the **static** keyword
        - ~~Abstract Classes~~
            - marked with abstract
            - cannot be instantiated directly
                - you have to inherit from them
                - they are just there to be inherited from
            - can provide basic setup that other, more specialized classes will need
            - you can also create abstract methods
                - you just create the type definition for the function, you don't actually implement any logic
                - abstract methods need to be overriden to give them their implementation by child classes
                    - this is required, you can't not implement the methods in the child class
        - Private Constructors and Singletons  
            // Private Constructors  
            class OnlyOne {  
                private static instance: OnlyOne;   // Can only be accessed from a method that is not private from outside. Also note that 'OnlyOne' here is a type!  
               
                private constructor(public name: string) {}     // Can only be accessed from a method that is not private from outside.  
               
                static getInstance() {  
                    if (!OnlyOne.instance) {    // If OnlyOne property of class OnlyOne does not exist than do below code...  
                        OnlyOne.instance = new OnlyOne('The Only One');     // New object named 'instance' will be created from Class OnlyOne with a property named 'name' containing the value of 'The Only One'.  
                    }  
                    return OnlyOne.instance;    // Here OnlyOne.instance will be a new object created from the Class OnlyOne.  
                }  
            }  
               
            let wrong = new OnlyOne('The Only One');  
            let right = OnlyOne.getInstance();      // Basically this line will create an object named instance as per the above comments, which will then be equal to a variable named 'right', therefore making right an Object Also!  
               
            console.log(right);     // This will be an object created from class OnlyOne with property name of 'name' with its value of 'The Only One'
        - ~~Read only properties~~
            - There are two ways of defining a property as read only
                - Defining a getter, but not a setter
                - Using the **readonly** keyword
                    - this allows you to set the property's value in the constructor, but nowhere else.
    - ~~Namespaces and Modules~~
        - ~~Introduction to Namespaces~~
            - Namespaces are like JavaScript objects
            - They are useful for containing things which you don't want clogging up the global scope
            - Create a new namespace using the **namespace** keyword
            - make functions defined in a namespace available from the outside using the **export** keyword
            - Access functions defined in a namespace using dot notation
        - ~~Namespaces and Multiple Files~~
            - You can break out a namespace accross multiple files by simply including the namespace delcaration in each file.
            - Compile to a single file by uisng --outfile when running the compiler
        - ~~Namespace Imports~~
            - Import a namespace with three forward slashes follwed by an angle bracket tag with the attribute "path" whose value is the file you want to import  
                /// <reference path="myNamespace.ts" />
            - When compiling using this method, you still need to specify the --outfile
            - Imports will be bundled in the same order in which they are specified
        - ~~More about namespaces~~
            - You can define namespaces inside of namespaces
            - You can assign a nested namespace an alias for easier access by using the **import** keyword
        - ~~Limitations of Namespaces~~
            - not very declarative about what dependencies they need
        - ~~Modules~~
            - export functions and variables that you want to use in other files using the **export** keyword
            - you can import things from other modules using **import { thing, otherThing } from "./path";**
            - When specifying the file path on an import statement, you should leave off the extension
                - the tsc will automatically look for .ts, .tsx, or .d.ts
        - ~~Loading Modules~~
            - When running code in the browser, you need to use a module loader because modules are not supported by ES5 and before
    - Doing Contract Work with Interfaces
        - ~~The basics about interfaces~~
            - An interface is a way to make sure that certain properties or methods are available on an object before you can do anything with it.
            - Create an interface using the **interface** keyword, followed by a model of the object containing all the required properties and methods
        - Interfaces and properties
            - ~~You can make a property in an interface optional if you add a ? after its name, before the colon.~~
            - ~~Object literals being passed to functions that use an interface are checked more strictly than variables which contain objects. They can only specify properties which exist in the interface.~~
            - If you don't know the name of a property when you're creating an interface, a placeholder name can be added in square brackets.  
                interface Dog {  
                name: string,  
                age: number,  
                [unknownProp: string]: any  
                }
        - ~~Interfaces and Methods~~
            - You add methods to an interface just like you would with methods on a class  
                interface Person {  
                greet (name: string): void;  
                }
        - ~~Using Interfaces with Classes~~
            - Make a class adhere to an interface by using the **implements** keyword  
                interface DogInterface {  
                name: string;  
                bark (): void;  
                }  
                  
                class Dog implements DogInterface {  
                name: string;  
                bark () {  
                console.log("Woof!");  
                }  
                }
        - ~~Interfaces and Function Types~~
            - interfaces can be used to describe functions as well  
                interface doubleValueFunction {  
                (value: number): number;  
                }  
                  
                let myFunction: doubleValueFunction;  
                myFunction (myNum: number) {  
                return myNum * 2;  
                }
            - The names of the parameters don't have to match, but the types of the parameters do
        - ~~Interface Inheritance~~
            - You can make an interface inherit from another interface using the **extends** keyword
        - ~~What happens when interfaces get compiled~~
            - Interfaces don't actually get compiled into JavaScript
            - Interfaces exist only to give you compilation errors, to check your code before it gets compiled.
    - ~~Generics~~
        - ~~Creating a generic function~~
            - A **type variable** is a special variable that works on types instead of values
                - Type variables are created using angle brackets
                - Type variables are a way of capturing information about the type of a parameter without declaring it explicitly.
                - This contrasts with the **any** type which explicitly says it's of type any
                - Without a type variable, you lose the information about what type a parameter was when the function returns
            - Create a generic function by writing your function name, then creating a type variable in brackets, then setting the type of the parameter (even the return value) equal to that type variable
        - ~~Generic Types and Arrays~~
            - You can tell a generic function to expect an array using a type variable by adding square brackets after the type variable
        - ~~Using Generic Types~~
            - You can use generic functions as a type assignment on a variable just like any other function.
        - ~~Generic Classes~~
            - You can create a generic class just like you create a generic function. Create a type variable just before the opening brace.
        - ~~Constraints~~
            - You can constrain which types can be used in a generic class by making your type variable **extend** another type or union of types.
        - ~~Using more than one generic type~~
            - You can add multiple generic types by separating them with commas
            - You can make one generic type **extend** another, which means that whatever type one is the other will also be.
            - Each generic type you add needs to have its own constraints, even if they are identical
    - ~~Decorators~~
        - ~~Creating a Class Decorator~~
            - Decorators provide a way to add both annotations and a meta-programming syntax for class declarations and members. 
            - Decorators are functions which receive a certain number of arguments depending on to what they are attached.
            - Decorators which are attached to classes receive one argument, which is the constructor function of the class.
            - You attach a decorator using the @ symbol followed by the name of the function
            - To make sure you don't get console warnings about decorators being an experimental feature, set "experimentalDecorators" to **true** in the tsconfig.json
        - ~~Decorator Factories~~
            - Decorator factories allow you to customize how a decorator is applied to a declaration
            - A Decorator Factory is simply a function that returns the expression that will be called by the decorator at runtime.
        - ~~Using multiple decorators~~
            - You can add multiple decorators to something by just listing them one after the other
        - ~~Method decorators~~
            - When using a decorator on a method, it takes 3 arguments
                - Either the constructor function of the class for a static member, or the prototype of the class for an instance member.
                    - Since this could be either of these, it should be of type **any**
                - The name of the member.
                - The Property Descriptor for the member.
        - ~~Property decorators~~
            - A decorator for a property takes two arguments
                - Either the constructor function of the class for a static member, or the prototype of the class for an instance member.
                    - should be of class **any**
                - The name of the member.
            - Property decorators do not have access to property descriptors on properties like method decorators do, this is due to technical limitations.
            - A property decorator can only be used to observe that a property of a specific name has been declared for a class.
        - ~~Parameter decorators~~
            - Parameter decorators take 3 arguments:
                - Either the constructor function of the class for a static member, or the prototype of the class for an instance member. (should be of type **any**)
                - The name of the member.
                - The ordinal index of the parameter in the function’s parameter list.
    - TypeScript Workflows
        - Using tsc and the tsconfig file
            - when using the tsc command, you can use the -w flag to ender watch mode
        - How TypeScript resolves files using tsconfig.json
            - Create a tsconfig file by using tsc --init
            - When you run the tsc command, TypeScript looks for the tsconfig.json file and reads it to learn how it should behave
                - The "exclude" node in the tsconfig tells TS what files should NOT be compiled
                - Alternatively, the "files" node can tell TS explicitly which files to compile
            - TypeScript looks for all .ts and .d.ts files
            - You can provide the tsc command with the name of the file you want to compile, in which case you can give the TSC compiler options by using their command line flags
            - If the tsconfig.json file does not live in the folder where the **tsc** command is being executed, you can pass the path to the tsconfig file using the -p flag followed by the path to the file