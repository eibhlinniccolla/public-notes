- you can save a custom type so that it can be reused using the **type** keyword  
- Then you can put this in a type declaration using the word chosen  

```ts
type StringObject = { 
	string1: string, 
	string2: string 
};

let newStringObject: StringObject = {  
	string1: "this is a string",  
	string2: "so is this"  
};
```

