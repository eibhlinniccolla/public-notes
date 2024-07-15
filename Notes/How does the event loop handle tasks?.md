How does the event loop handle tasks?
	- The JS engine organizes tasks into queues, one for microtasks and one for macrotasks
		- Like all queues, the macro- and microtask queues are first-in-first-out
	- Macrotask queue (a.k.a. Callback Queue, Message Queue, or Task Queue)
		- Each turn of the event loop handles one macrotask
		- Macrotasks are handled **after** any synchronous code is executed (i.e. once the call stack is empty)
		- If any new macrotasks were queued during the handling of the current one, they are added to the end of the macrotask queue
		- When a macrotask is completed, it is removed from the queue and the next macrotask in line is executed (if there is one)
	- Microtask queue (a.k.a. Job Queue)
		- During the processing of each macrotask, microtasks may be added to the microtask queue
			- Furthermore, microtasks can also queue up other microtasks
		- Any additional microtasks queued during microtasks are added to the end of the queue and also processed.
		- Before the event loop moves onto the next macrotask, all microtasks must have been completed
			- This is true even if a microtask queues up other microtasks, which can cause the macrotask queue to never be processed
				- this is called "starving" the macrotask queue
	- The algorithm looks something like this:
		1. Dequeue and run the oldest task from the _macrotask_ queue (e.g. “script”).
		2. Execute all _microtasks_:
			- While the microtask queue is not empty:
				- Dequeue and run the oldest microtask.
		3. Render changes if any.
		4. If the macrotask queue is empty, wait till a macrotask appears.
		5. Go to step 1.