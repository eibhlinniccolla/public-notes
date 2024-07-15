Async iterators
	- [Fasy Library](https://github.com/getify/fasy) gives eager async iterators that let you iterate over promises
- Problems with Async functions
	- Only consumes thenables (promises)
	- Scheduling (Starvation, microtask queue)
		- https://github.com/nodejs/promise-use-cases/issues/25
		- https://en.wikipedia.org/wiki/Communicating_sequential_processes
		- https://everipedia.org/wiki/lang_en/Starvation_(computer_science)
		- https://stackoverflow.com/questions/1162587/what-is-starvation
		- https://www.differencebetween.com/difference-between-deadlock-and-vs-starvation/
	- Cannot cancel them externally
		- https://www.youtube.com/watch?v=VDaKLQE03ss
		- https://github.com/getify/caf
- Under the hood, `async` `await` is just syntactic sugar for a generator that `yield`s a promise
	- This:  
		```  
		async function logTweet () {  
		var tweet = await fetch("https://twitter.com/eileen/1");  
		console.log(tweet);  
		}  
		  
		logTweet();  
		```
	- Is essentially this:  
		```  
		function *logTweet () {  
		var tweet = yield fetch("https://twitter.com/eileen/1");  
		console.log(tweet);  
		}  
		  
		var iterator = logTweet();  
		var yieldedPromise = iterator.next();  
		yieldedPromise.then((tweet) => iterator.next(tweet));  
		```
- Async generators with yield
	- Allow you to use `yield` as a pushing mechanism, and `await` as a pulling mechanism
	- Example  
		`async function* fetchURLs (urls) {  
		for (let url of urls) {  
		let res = await fetch(url);  
		if (res.status == 200) {  
		let text = await res.text();  
		yield text.toUpperCase();  
		} else {  
		yield undefined;  
		}  
		}  
		}`
	- Consuming asynchronous generators
		- You're getting back a promise with an generator result from the asynchronous generator, so you can't iterate over it normally using `...` or `for ... of` because you don't know if you're done until you get the value out of the promise
		- This is why you have to use a `for await ... of` loop
			- Syntactic sugar for waiting for the promise with the current iteration to resolve before consuming the result of the iteration
			- Example  
				`async function main (favoriteSites) {  
				for await (let text of fetchURLs(favoriteSites)) {  
				console.log(text);  
				}  
				}