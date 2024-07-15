You can specify the format (type) of a function  
- specify what parameters it should be able to accept in parens
- specify what its return value should be using arrow syntax

```ts
let myFunction: (a: number, b: number) => number;

myFunction = function (a, b) {
	return a + b;
}

myFunction("1", "2"); // compiler will error
```

