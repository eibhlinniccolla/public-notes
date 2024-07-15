The event loop is a conceptual model for understanding how the JavaScript engine deals with [[Tasks (JavaScript)|tasks]]. The event loop follows a very simple algorithm:

1. While there are tasks, execute them starting with the oldest task.
2. Sleep until a task appears, then go to 1.

The event loop runs continually, executing any tasks queued, however it does nothing most of the time, taking up close to 0 CPU, and only takes action if there is a task to process.

Each 'thread' gets its own **event loop**, so each web worker gets its own, so it can execute independently, whereas all windows on the same origin share an event loop as they can synchronously communicate.

- [[How does the event loop handle tasks?]]
- Why use an event loop?
	- [[JavaScript is Synchronous]]
	- [[JavaScript is Single-Threaded]]