---
published: true
---
JavaScript is [[Thread (Software Engineering)|single-threaded]], so if an asynchronous task needs to be completed, the JS engine can't simply spin up another thread to handle it. It has to wait until the task is completed. This is why handling I/O is typically performed via [[Event (JavaScript)|events]] and [[Callback Function|callbacks]], so when the application is waiting for an async operation to finish, it can still process other things like user input.