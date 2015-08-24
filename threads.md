# Threads — Threat or Menace?

Though Unix developers have long been comfortable with computation by multiple cooperating processes, they do not have a native tradition of using threads (processes that share their entire address spaces). These are a recent import from elsewhere, and the fact that Unix programmers generally dislike them is not merely accident or historical contingency.

From a complexity-control point of view, threads are a bad substitute for lightweight processes with their own address spaces; the idea of threads is native to operating systems with expensive process- spawning and weak IPC facilities.

By definition, though daughter threads of a process typically have separate local-variable stacks, they share the same global memory. The task of managing contentions and critical regions in this shared address space is quite difficult and a fertile source of global complexity and bugs. It can be done, but as the complexity of one’s locking regime rises, the chance of races and deadlocks due to unanticipated interactions rises correspondingly.

Threads are a fertile source of bugs because they can too easily know too much about each others’ internal states. There is no automatic encapsulation, as there would be between processes with separate address spaces that must do explicit IPC to communicate. Thus, threaded programs suffer from not just ordinary contention problems, but from entire new categories of timing-dependent bugs that are excruciatingly difficult to even reproduce, let alone fix.

Thread developers have been waking up to this problem. Recent thread implementations and standards show an increasing concern with providing thread-local storage, which is intended to limit problems arising from the shared global address space. As threading APIs move in this direction, thread programming starts to look more and more like a controlled use of shared memory.