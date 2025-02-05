# preemptible-thread

[This idea comes from inspiration from Stephen from this Stackoverflow question : how to preempt a thread from userspace](https://stackoverflow.com/questions/72625285/preempt-a-hot-loop-thread-from-user-space)

How to preempt threads in user space?

You set the looping variable to the limit! You can therefore implement an M:N scheduler with this pattern. I've written this multiplexer in Java, C and Rust.

This is a userspace thread scheduler. It switches between green threads very fast as frequently as you want it to.

This scheduler cannot preempt arbitrary code, it requires no cooperation at descheduling time - only you register the memory locations of the loop variable.

These code examples show preempting a hot loop in a green thread in Rust, C and Java.

Rather than introducing an if statement into every iteration of the loop that slows down the loop, we update the loop invariants to cancel the loop. This approach can be used to create cancellable APIs.

# preemptible-thread-evented.c

This is preeemptible threads with threads for IO events. Normally you would use libev or libuv but I thought I could combine my preemptible green threads with IO scheduled on IO threads. 

Multiple green threads schedule events to read from the file system and the IO threads handle the blocking reads.

I should probably change IO callbacks to be enqueued onto the RingBuffer as they could currently run at the same time as a lightweight user thread.
