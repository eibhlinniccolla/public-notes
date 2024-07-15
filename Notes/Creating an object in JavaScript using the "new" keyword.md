
## Example:

```js
function User (name, score) {  
	this.name = name;  
	this.score = score;  
}  
  
User.prototype.increment = function () {  
	this.score++;  
}  
  
var user = new User("Eileen", 3);  
  
user.name; // "Eileen"  
user.score; // 3  
user.increment();  
user.score; // 4  
```