Tasks are bits of code or functionality that the JavaScript engine must execute

- A task contains 2 things
	- an associated [[function (JavaScript)|function]] which is executed when the task is processed
	- a message which is passed to the function as input
- A task's associated function retains a reference to the [[variable environment]] that it was created in via [[Closure]]

- Where to tasks come from?
	- Tasks can come from many sources, for example:
		- External scripts (`<script src="...">`)
			- The task is to execute the script
		- User input
			- When a user moves their mouse, the task is to dispatch a `mousemove` event and execute its handlers
		- `setTimeout` (and other async browser APIs)
			- The task is to execute the callback supplied to `setTimeout`
		- Promises resolving
			- The task is to update the `.value` of the promise, and execute any functions added via `.then` or `.catch`
- What purpose do tasks serve?
	- **Tasks** are a way for the browser to get from its internals into JavaScript/DOM land and ensures these actions happen sequentially.
- Types of tasks

- Processing tasks
	- When a task is processed, any data associated with it (e.g. the cursor's x and y values in a `mousemove` event) are passed to the associated function
	- Like all functions, a task's associated function creates a new stack frame on the callstack for its use
	- If a task takes too long, the browser can’t do other tasks, such as processing user events. So after a time, it raises an alert like “Page Unresponsive”, suggesting killing the task with the whole page. That happens when there are a lot of complex calculations or a programming error leading to an infinite loop.