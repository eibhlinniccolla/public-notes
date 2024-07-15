---
published: true
---
- The `new` keyword does 5 things
	- It creates a new object. The type of this object is simply `'object'`.
	- It sets this created object's internal, inaccessible, `[[prototype]]` property to be the constructor function's external, accessible, `.prototype` property
	- It makes the `this` keyword point to the newly created object.
	- It executes the constructor function, using the newly created object whenever `this` is mentioned.
	- It returns the newly created object (unless the constructor function returns a non-`null` object reference. In this case, that object reference is returned instead).

The `new` keyword is simply syntactic sugar for behavior you could code from scratch. It simply allows our constructor functions to be shorter.
        - Example
            - With `new` keyword  
                ```function User (name) {  
                this.name = name;  
                }  
                  
                User.prototype.sayHello = function() {  
                console.log('Hi my name is ' + this.name);  
                }  
                  
                const newUser = new User('Eileen');  
                  
                newUser.sayHello(); // Hi my name is Eileen  
                ```
            - Without `new` keyword  
                ```function User (name) {  
                var user = Object.create(User.prototype);  
                user.name = name;  
                return user;  
                }  
                  
                User.prototype.sayHello = function() {  
                console.log('Hi my name is ' + this.name);  
                }  
                  
                var newUser = User('Eileen');  
                  
                newUser.sayHello(); // Hi my name is Eileen  
                ```