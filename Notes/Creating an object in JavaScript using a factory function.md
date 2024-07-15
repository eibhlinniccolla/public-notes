
## Example:

```js  
function userCreator (name, score) {  
	var newUser = Object.create(null);  
	newUser.name = name;  
	newUser.score = score;  
	newUser.increment = function () {  
		newUser.score++;  
	}  
	return newUser;  
}  
  
var user = userCreator("Eileen", 3);  
user.name; // "Eileen"  
user.score; // 3  
user.increment();  
user.score; // 4  
```
