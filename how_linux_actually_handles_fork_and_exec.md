# How Linux actually handles fork and exec

#### clone

In the kernel, fork is actually implemented by a clone system call. This clone interfaces effectively provides a level of abstraction in how the Linux kernel can create processes.

clone allows you to explicitly specify which parts of the new process are copied into the new process, and which parts are shared between the two processes. This may seem a bit strange at first, but allows us to easily implement threads with one very simple interface.

### Threads

While fork copies all of the attributes we mentioned above, imagine if everything was copied for the new process except for the memory. This means the parent and child share the same memory, which includes program code and data.