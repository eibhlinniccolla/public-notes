- introduce a declaration using `--`
- value of a property may be any valid CSS value:
	- color
	- string
	- layout value
	- expression
- You can't have a global variable that declares a custom property outside of a selector, use `:root` instead  

```css
:root {  
	--globalVar: 10px;  
}  
  
.enclosing {  
	--enclosingVar: 20px;  
}  

.enclosing .closure {  
	--closureVar: 30px;  
	font-size: calc(var(--closureVar) + var(--enclosingVar) + var(--globalVar);  
}
```
- custom CSS properties are inherited by default, and cascade