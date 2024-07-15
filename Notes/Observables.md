Observables, RxJS
- https://medium.com/@benlesh/hot-vs-cold-observables-f8094ed53339
- https://medium.com/@benlesh/on-the-subject-of-subjects-in-rxjs-2b08b7198b93
- https://alligator.io/rxjs/subjects/
- Pull- vs. Push-based systems
	- _Pull_ and _Push_ are two different protocols that describe how a data _Producer_ can communicate with a data _Consumer_.
	- Pull (Passive producer, Active consumer)
		- In Pull systems, the Consumer determines when it receives data from the data Producer.
		- The Producer itself is unaware of when the data will be delivered to the Consumer.
		- Every JavaScript Function is a Pull system. The function is a Producer of data, and the code that calls the function is consuming it by "pulling" out a _single_ return value from its call.
		- ES2015 introduced generator functions and iterators (function*), another type of Pull system. Code that calls iterator.next() is the Consumer, "pulling" out multiple values from the iterator (the Producer).
	- Push (Active producer, Passive consume)
		- In Push systems, the Producer determines when to send data to the Consumer. The Consumer is unaware of when it will receive that data.
		- Promises are the most common type of Push system in JavaScript today.
			- A Promise (the Producer) delivers a resolved value to registered callbacks (the Consumers), but unlike functions, it is the Promise which is in charge of determining precisely when that value is "pushed" to the callbacks.
		- An Observable is a Producer of multiple values, "pushing" them to Observers (Consumers).
	- In summary:
		- Pull
			- Single: Functions
			- Multiple: Iterators
		- Push
			- Single: Promise
			- Multiple: Observable
- `Observable`
	- A representation of any set of values over any amount of time.
	- The `Observable` constructor function accepts a function which is initally called when the function is subscribed to
		- This function is passed a `Subscriber`
	- `Observable.subscribe()` and the `subscribe()` function passed to the `Observable` constructor are not the same in the library, but may be considered conceptually equal.
	- Subscribing to an observable
		- The `subscribe()` method takes either a `next` function or an `Observer` object and returns a `Subscription`
		- Don't pass individual callbacks for `next`, `error`, and `complete`
		- cancelling a subscription will not call complete callback provided to subscribe function, which is reserved for a regular completion signal that comes from an Observable.
		- The `subscribe()` method calls `Observable`'s internal subscribe function
			- The internal subscribe function is either the one passed in the the constructor, or (as is most often the case) a library implementation
		- Calling `subscribe()` is actually the moment when Observable starts its work, not when it is created, as it is often the thought.
		- callbacks provided to subscribe are not guaranteed to be called asynchronously. It is an Observable itself that decides when these functions will be called.
			- For example `of()` by default emits all its values synchronously.
	- `forEach()`
		- Used as a NON-CANCELLABLE means of subscribing to an observable, for use with APIs that expect promises, like `async`/`await`.
		- You cannot unsubscribe from this.
		- Example:  
			```  
			var source$ = interval(1000).pipe(take(4));  
			  
			async function sum4 () {  
			var total = 0;  
			  
			await source$.forEach(val => total += val);  
			  
			return total;  
			}  
			  
			getTotal.then(console.log); // 6  
			```
	- `toPromise()`
		- Subscribe to this Observable and get a Promise resolving on complete with the last emission (if any).
	- Only use `forEach` and `toPromise` with observables you know will complete.
		- If the source observable does not complete, you will end up with a promise that is hung up, and potentially all of the state of an async function hanging out in memory.
		- To avoid this situation, look into adding something like `timeout`, `take`, `takeWhile`, or `takeUntil` amongst others.
- `Observer`
	- `Observer` is the public API for consuming values from an `Observable`
	- The `Observer` interface has 3 methods
		- `next: (value: T) => void`
			- Called when the source observable emits a value
		- `complete: () => void`
			- Called when the source observable completes
		- `error: (err: any) => void`
			- Called when the source observable throws an error
	- All Observers get converted to a `Subscriber`, in order to provide Subscription-like capabilities such as `unsubscribe()`.
	- `Observable` class takes care of the creation of "safe" `Observer`s, which means that it can be applied to passed anonymous observer functions
	- "Safe" `Observer` Guarantees
		- If you pass an Observer doesn’t have all of the methods implemented, that’s okay.
		- You don’t want to call `next` after a `complete` or an `error`
		- You don’t want anything called if you’ve unsubscribed.
		- Calls to `complete` and `error` need to call unsubscription logic.
		- If your `next`, `complete` or `error` handler throws an exception, you want to call your unsubscription logic so you don’t leak resources.
		- `next`, `error` and `complete` are all actually optional. You don’t have to handle every value, or errors or completions. You might just want to handle one or two of those things.
			- Note however, if the error method is not provided and an error happens, it will be thrown asynchronously. Errors thrown asynchronously cannot be caught using try/catch.
	- Example:
		- Creating an observer manually  
			```  
			var source$ = from([1,2,3]);  
			  
			var observer = {  
			next(value) { console.log(value); }  
			};  
			  
			source$.subscribe(observer); // 1, 2, 3  
			```
- `Subscriber`
	- An object which implements the `Observer` interface and extends the `Subscription` class
	- Do not create instances of the `Subscriber` class directly
	- Extending the `Subscriber` class
		- The `Subscriber` constructor accepts 1 argument
			- `destination`
				- Contains a reference to another `Subscriber` or an `Observer`
		- There are 3 protected internal methods which are responsible for calling their public equivalent when the `Subscriber` receives a notification from the source observable
			- `_next()`
			- `_complete()`
			- `_error()`
		- These properties can be overriden to change their behavior by extending the class
		- Example
			- `DoubleSubscriber`  
				```  
				var source$ = from([1,2,3,4]);  
				  
				var observer = {  
				next(value) {  
				console.log(value); // 2, 4, 6, 8  
				}  
				}  
				  
				class DoubleSubscriber extends Subscriber {  
				_next(value) {  
				this.destination.next(value * 2);  
				}  
				}  
				  
				source$.subscribe(new DoubleSubscriber(observer));  
				```
- Producers
	- A producer is the source of values for an observable
	- A producer can be something like a web socket, a DOM event, an iterator, or something looping over an array.
- Operators
	- An operator is a function that takes a source observable and returns a new observable that will subscribe to the source observable when you subscribe to it.
	- Building operator chains is really just creating a template for wiring together observers on subscription'
- Subjects
	- Behavior Subjects
	- Replay Subjects
- "Hot" vs. "Cold"
	- **TL;DR:** You want a HOT observable when you don’t want to create your producer over and over again.
	- **Cold** is when your observable creates the producer
	- **Hot** is when your observable closes over the producer
- "Unicasting" vs "Multicasting"