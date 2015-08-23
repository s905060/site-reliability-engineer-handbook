# OPEN KNOWLEDGE - My SRE Handbook

**(i)** Make each program do one thing well. To do a new job, build afresh rather than complicate old programs by adding new features.

**(ii)** Expect the output of every program to become the input to another, as yet unknown, program. Don't clutter output with extraneous information. Avoid stringently columnar or binary input formats. Don't insist on interactive input.

**(iii)** Design and build software, even operating systems, to be tried early, ideally within weeks. Don't hesitate to throw away the clumsy parts and rebuild them.

**(iv)** Use tools in preference to unskilled help to lighten a programming task, even if you have to detour to build the tools and expect to throw some of them out after you've finished using them.

This is the Unix philosophy: 
* Write programs that do one thing and do it well. 
* Write programs to work together. 
* Write programs to handle text streams, because that is a universal interface.

**@Doug McIlroy**

---

**Rule 1.** You can't tell where a program is going to spend its time. Bottlenecks occur in surprising places, so don't try to second guess and put in a speed hack until you've proven that's where the bottleneck is.

**Rule 2.** Measure. Don't tune for speed until you've measured, and even then don't unless one part of the code overwhelms the rest.

**Rule 3.** Fancy algorithms are slow when n is small, and n is usually small. Fancy algorithms have big constants. Until you know that n is frequently going to be big, don't get fancy. (Even if n does get big, use Rule 2 first.)

**Rule 4.** Fancy algorithms are buggier than simple ones, and they're much harder to implement. Use simple algorithms as well as simple data structures.

**Rule 5.** Data dominates. If you've chosen the right data structures and organized things well, the algorithms will almost always be self-evident. Data structures, not algorithms, are central to programming.

**Rule 6.** There is no Rule 6.

**@Rob Pike**

---

When in doubt, use brute force.

**@Ken Thompson**

---

August, 2015

---
###### ** All contents/examples are copied from internet. **