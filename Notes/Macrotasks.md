- What is a macrotask?
	- any JavaScript scheduled to be run by the standard mechanisms such as initially starting to execute a program, an event triggering a callback, and so forth
- Examples of macrotasks
	- Executing a script
	- `setTimeout`
	- `setInterval`
	- `setImmediate`
	- `requestAnimationFrame`
	- I/O
	- UI Rendering
- Macrotasks are the default way code is executed
	- Even when you run a script containing fully synchronous code, a new macrotask is added to the macrotask queue to run the code in the script
- Breaking work up across multiple tasks
	- You can use `setTimeout` as a way to queue up a new macrotask
	- Use-cases
		- splitting up CPU-hungry tasks
		- progress indication
		- doing something after an event fully bubbles up and is handled

[[Macrotask Queue]]