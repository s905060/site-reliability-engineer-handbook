# Threads — Threat or Menace?

Though Unix developers have long been comfortable with computation by multiple cooperating processes, they do not have a native tradition of using threads (processes that share their entire address spaces). These are a recent import from elsewhere, and the fact that Unix programmers generally dislike them is not merely accident or historical contingency.

From a complexity-control point of view, threads are a bad substitute for lightweight processes with their own address spaces; the idea of threads is native to operating systems with expensive process- spawning and weak IPC facilities.

By definition, though daughter threads of a process typically have separate local-variable stacks, they share the same global memory. The task of managing contentions and critical regions in this shared address space is quite difficult and a fertile source of global complexity and bugs. It can be done, but as the complexity of one’s locking regime rises, the chance of races and deadlocks due to unanticipated interactions rises correspondingly.

Threads are a fertile source of bugs because they can too easily know too much about each others’ internal states. There is no automatic encapsulation, as there would be between processes with separate address spaces that must do explicit IPC to communicate. Thus, threaded programs suffer from not just ordinary contention problems, but from entire new categories of timing-dependent bugs that are excruciatingly difficult to even reproduce, let alone fix.

Thread developers have been waking up to this problem. Recent thread implementations and standards show an increasing concern with providing thread-local storage, which is intended to limit problems arising from the shared global address space. As threading APIs move in this direction, thread programming starts to look more and more like a controlled use of shared memory.

To add insult to injury, threading has performance costs that erode its advantages over conventional process partitioning. While threading can get rid of some of the overhead of rapidly switching process contexts, locking shared data structures so threads won’t step on each other can be just as expensive.

This problem is fundamental, and has also been a continuing issue in the design of Unix kernels for symmetric multiprocessing. As your resource-locking gets finer-grained, latency due to locking overhead can increase fast enough to swamp the gains from locking less core memory.

One final difficulty with threads is that threading standards still tend to be weak and underspecified as of mid-2003. Theoretically conforming libraries for Unix standards such as POSIX threads (1003.1c) can nevertheless exhibit alarming differences in behavior across platforms, especially withrespecttosignals,interactionswithotherIPCmethods,andresourcecleanuptimes. Windows and classic MacOS have native threading models and interrupt facilities quite different from those of Unixandwilloftenrequireconsiderableportingeffortevenforsimplethreadingcases. Theupshot is that you cannot count on threaded programs to be portable.

For more discussion and a lucid contrast with event-driven programming, see Why Threads Are a Bad Idea [Osterhout96].
