# Caching

Sometimes you can get the best of both worlds (low latency and good throughput) by computing expensive results as needed and caching them for later use. Earlier we mentioned that named reduces latency by batching; it also reduces latency by caching the results of previous network transactions with other DNS servers.

Caching has its own problems and tradeoffs, which are well illustrated by one application: the use ofbinarycachestoeliminateparsingoverheadassociatedwithtextdatabasefiles. Somevariantsof Unix have used this technique to speed up access to their password information (the usual motivation was to cut latency on logins at very large sites).

To make this work, all code that looks at the binary cache has to know that it should check the timestamps on both files and regenerate the cache if the text master is newer. Alternatively, all changes to the textual master must be made through a wrapper that will update the binary format.

While this approach can be made to work, it has all the disadvantages that the SPOT rule would lead us to expect. The duplication of data means that it doesn’t yield any economy of storage — it’s purely a speed optimization. But the real problem with it is that the code to ensure coherency between cache and master is notoriously leaky and bug-prone. Very frequently updated cache files can lead to subtle race conditions simply because of the 1-second resolution of timestamps.

Coherency can be guaranteed in simple cases. One such is the Python interpreter, which compiles and deposits on disk a p-code file with extension .pyc when a Python library file is first imported. On subsequent runs the cached copy of the p-code is loaded unless the source has since changed (this avoids reparsing the library source code on every run).
Emacs Lisp uses a similar technique with .el and .elc files. This technique works because both read and write accesses to the cache go through a single program.

When the update pattern of the master is more complex, however, the synchronization code tends to spring leaks. The Unix variants that used this technique to speed up access to critical system databases were infamous for spawning system-administrator horror stories that reflected this.

Ingeneral,binary cache files are a brittle technique and probably best avoided. The work that went into implementing a special-purpose hack to reduce latency in this one case would have been better spent improving the application design so it doesn’t have a bottleneck there — or even on tuning to improve the speed of the file system or the virtual-memory implementation.

When you think you are in a situation that demands caching, it is wise to look one level deeper and ask why the caching is necessary. It may well be no more difficult to solve that problem than it would be to get all the edge cases in the caching software right.