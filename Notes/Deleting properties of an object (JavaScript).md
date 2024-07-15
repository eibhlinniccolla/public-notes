To delete a property from an object, use the `delete` operator
	- The `delete` operator returns `true` if the delete was successful
	- The `delete` operator also returns `true` if the property to delete was nonexistent
- You can only use the `delete` keyword to delete an object's **own** properties (i.e. not inherited ones)
	- You must delete inherited properties on the prototype object (where they were defined)
- You can't delete properties with their `configurable` attribute set to `false`
- You also cannot delete properties of the global object, which were declared with the `var` keyword.

## Example:
```js
var foo = {  
	a: 1
};  

var bar = Object.create(foo);  
bar.b = 2;  

Object.defineProperty(bar, "d", {  
	value: 4  
});  

Object.defineProperty(bar, "e", {  
	value: 5,  
	configurable: true  
});  

bar.a; // 1  
delete bar.a; // true  
bar.a; // 1
delete foo.a; // true  
foo.a; // undefined  
bar.a; // undefined  

bar.b; // 2  
delete bar.b; // true  
bar.b; // undefined  

bar.c; // undefined  
delete bar.c; // true  
bar.c; // undefined  

bar.d; // 4  
delete bar.d; // false  
bar.d; // 4  

bar.e; // 5  
delete bar.e; // true  
bar.e; // undefined  
```