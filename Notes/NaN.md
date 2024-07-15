- `NaN` represents an invalid number, or the result of an invalid numerical operation
    - `NaN` when used with any other mathematical operator **always** returns `NaN`
    - `NaN` is the only value in JavaScript which does not have the **identity property** (a value is equal to itself)  
        `NaN === NaN; // false`
    - `typeof NaN` returns `number`, because `NaN` is **not** "not a number", but rather it is an **invalid** number
- `NaN` does not signify that something is simply not a number, rather it is the result of an invalid numerical operation
    - `Number.isNaN('hello'); // false`
    - `Number.isNaN('hello' / 12); // true`
- `isNaN()` returns whether or not a value is `NaN`
    - **important** it also coerces the value to a number before checking if it's `NaN`  
        `isNaN(4); // false  
        isNaN(3 / 'hello'); // true  
        isNaN('hello'); // true`
    - `Number.isNaN()` does the same thing as `isNaN()` **except** it does NOT coerce the value to a number first  
        `Number.isNaN(3 / 'hello'); // true  
        Number.isNaN('hello'); // false`
    - manual way to check if number is `NaN`  
        `function customIsNan(v) {  
        return v !== v;  
        }`