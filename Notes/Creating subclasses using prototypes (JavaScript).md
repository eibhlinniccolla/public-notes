---
published: true
---
## Using factory functions:

```js
function createUser (name) {  
	var user = Object.create(userFunctions);  
	user.name = name;  
	return user;  
}  
  
var userFunctions = {  
	sayName: function () {  
		console.log(this.name);  
	}  
}  
  
function createPaidUser (name, balance) {  
	var paidUser = createUser(name);  
	Object.setPrototypeOf(paidUser, paidUserFunctions);  
	paidUser.balance = balance;  
	return paidUser;  
}  
  
var paidUserFunctions = {  
	addBalance: function (balanceToAdd) {  
		this.balance += balanceToAdd;  
	}  
}  
  
Object.setPrototypeOf(paidUserFunctions, userFunctions);  
  
var paidUser = createPaidUser("Eileen", 0);  
  
paidUser.sayName(); // "Eileen"  
paidUser.addBalance(50);  
console.log(paidUser.balance); // 50  
```

## Using the `new` keyword:

```js
function User (name) {  
	this.name = name;  
}  
  
User.prototype.sayName = function () {  
	console.log(this.name);  
}  
  
function PaidUser (name, balance) {  
	User.call(this, name);  
	this.balance = balance;  
}  
  
Object.setPrototypeOf(PaidUser.prototype, User.prototype);  
// OR PaidUser.prototype = Object.create(User.prototype);  
  
PaidUser.prototype.addBalance = function (balanceToAdd) {  
	this.balance += balanceToAdd;  
}  
  
var paidUser = new PaidUser("Eileen", 0);  
  
paidUser.sayName(); // "Eileen"  
paidUser.addBalance(50);  
console.log(paidUser.balance); // 50  
  
```