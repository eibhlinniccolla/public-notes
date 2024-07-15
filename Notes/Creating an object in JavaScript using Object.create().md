
## Example:

```js  
var user = Object.create(null);  
user.name = "Eileen";  
user.score = 3;  
user.increment = function () { obj.score++ }  
  
user.name; // "Eileen"  
user.score; // 3  
user.increment();  
user.score // 4  
```