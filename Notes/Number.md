---
published: true
---
Javascript does not make a distinction between numbers that have decimals and numbers that don't
## `-0`:

```-0``` has lots of weird behaviors  

```js
var trendRate = -0;
  
trendRate === -0; // true  
trendRate.toString(); // "0"  
trendRate === 0; // true  
trendRate < 0; // false  
trendRate > 0; // false
```

The only built-in way to check if a value is `0` vs. `-0` is by using `Object.is()`  

```js
Object.is(trendRate, -0); // true
```

You can't use `Math.sign()` to see if a value is `0` or `-0` because it returns those values instead of `1` or `-1`

workaround:

```js
function sign(v) {  
	return v !== 0  
		? Math.sign(v)  
		: Object.is(v, -0)  
			? -1  
			: 1;  
}
```

manual way to check if number is `-0`  

```js
function isItNegativeZero(v) {  
	return v === 0 && (1 / v) === -Infinity;  
}

0 === -0; // true
1 / 0 // Infinity
1 / -0 // -Infinity
```

[Infinity keyword](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Infinity)