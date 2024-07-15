
## Example:

```js
var book = {
	title: "Ways to Go", 
	pages: 280, 
	bookMark1:"Page 20"
};

console.log ( book["title"]); //Ways to Go​  
console.log ( book["pages"]); // 280​  
​  
​//Or, in case you have the property name in a variable:​  
​var titleProperty = "title";  
console.log ( book[titleProperty]); // Ways to Go​

// Any expression that evaluates to a string can be used as a key
console.log (book["bookMark" + 1]); // Page 20
```

