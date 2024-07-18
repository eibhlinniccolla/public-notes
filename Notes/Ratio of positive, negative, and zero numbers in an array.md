```javascript
function printRatios (array) {
	var length = array.length;
	var positives = 0;
	var negatives = 0;
	var zeroes = 0;

	array.forEach(function updateCount (num) {
		if (num > 0) {
			positives += 1;
		} else if (num < 0) {
			negatives += 1;
		} else {
			zeroes += 1;
		}
	});

	console.log((positives / length).toFixed(6));
	console.log((negatives / length).toFixed(6));
	console.log((zeroes / length).toFixed(6));
}
```

