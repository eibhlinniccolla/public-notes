---
published: true
---
An object does not "inherit" functionality from its prototype, in the sense that the functionality is not copied into the object.
- Rather, its prototype is a _reference_ to another object which has the desired functionality.  
```js
var obj1 = {  
funky: function () {  
console.log('foo');  
}  
}  
  
var obj2 = Object.create(obj1);  
  
console.log(Object.getPrototypeOf(obj2).funky === obj1.funky); // true```
