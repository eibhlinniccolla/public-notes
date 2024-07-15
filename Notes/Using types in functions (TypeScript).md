Types can be used to:
### Declare the return type of a function
```ts
let x = "my string";  
function myFunction(): string {  
	return x;  
}
```

Use the `void` keyword when a function will not return a value
```ts
function sayHi(): void {  
	console.log("hi");  
}
```
### Declare the types of a function's parameters
```ts
function foo (bar: string): void {  
	console.log(bar);  
}
```

