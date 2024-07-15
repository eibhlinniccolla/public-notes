---
published: true
---
- **always** returns an empty object
- sets the prototype of the returned object to the object that you pass as an argument

```js
var obj1 = {};  
var obj2 = Object.create(obj1);  
  
console.log(Object.getPrototypeOf(obj2) === obj1); // true  
```