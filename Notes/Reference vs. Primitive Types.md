- Reference data types' values are stored as a reference, not directly on the variable as a value.
    - Example:  
        // The primitive data type String is stored as a value​  
        ​var person = "Kobe";  
        ​var anotherPerson = person; // anotherPerson = the value of person​  
        person = "Bryant"; // value of person changed​  
        ​  
        console.log(anotherPerson); // Kobe​  
        console.log(person); // Bryant
        - Even though _person_ was changed to "Bryant", the _anotherPerson_ variable still retains the value that _person_ had.
    - Example:  
        var person = {name: "Kobe"};  
        ​var anotherPerson = person;  
        person.name = "Bryant";  
        ​  
        console.log(anotherPerson.name); // Bryant​  
        console.log(person.name); // Bryant
        - In this example, we copied the person object to anotherPerson, but because the value in person was stored as a reference and not an actual value, when we changed the person.name property to “Bryant” the anotherPerson reflected the change because it never stored an actual copy of it’s own value of the person’s properties, it only had a reference to it.
- Primitive data types are immutable whereas Objects are mutable
- When you assign the value of an object's property to a variable, and that value is a primitive type, the variable does not retain a reference to that property.  
    `const myObj = { name: 'Eileen' };  
    const myVar = myObj.name; // 'Eileen'  
    myObj.name = 'Natasha';  
    console.log(myVar); // 'Eileen'`